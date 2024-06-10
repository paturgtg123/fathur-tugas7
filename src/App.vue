<template>
  <div id="app" class="container">
    <h1>Daftar Lagu Favorit</h1>
    <div class="input-container">
      <input 
        v-model="newTodo" 
        placeholder="Tambahkan Lagu Favorit Anda" 
        @keyup.enter="addNewTodo" 
        class="todo-input"
      />
      <button @click="addNewTodo" class="add-button">Add</button>
    </div>
    <ul class="todo-list">
      <li 
        v-for="(todo, index) in todos" 
        :key="index" 
        class="todo-item"
      >
        <span 
          :class="{ completed: todo.completed }" 
          @click="toggleCompletion(index)"
          class="todo-title"
        >
          {{ todo.title }}
        </span>
        <button @click="confirmRemove(index)" class="delete-button">Delete</button>
      </li>
    </ul>
    <p>Lagu Yang Belum Anda Dengarkan: {{ incompleteTodosCount }}</p>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import { defineStore, createPinia } from 'pinia'

const useTodoStore = defineStore('todo', {
  state: () => ({
    todos: []
  }),
  actions: {
    addTodo(title) {
      this.todos.push({ title, completed: false })
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
    },
    toggleTodoCompletion(index) {
      this.todos[index].completed = !this.todos[index].completed
    }
  },
  getters: {
    incompleteTodosCount: (state) => {
      return state.todos.filter(todo => !todo.completed).length
    }
  }
})

const pinia = createPinia()

export default {
  name: 'App',
  setup() {
    const newTodo = ref('')
    const todoStore = useTodoStore(pinia)

    const addNewTodo = () => {
      if (newTodo.value.trim() !== '') {
        todoStore.addTodo(newTodo.value)
        newTodo.value = ''
      }
    }

    const remove = (index) => {
      todoStore.removeTodo(index)
    }

    const toggleCompletion = (index) => {
      todoStore.toggleTodoCompletion(index)
    }

    const confirmRemove = (index) => {
      if (confirm('Apakah Anda yakin ingin menghapus lagu ini?')) {
        remove(index)
      }
    }

    const incompleteTodosCount = computed(() => todoStore.incompleteTodosCount)

    return {
      newTodo,
      todos: todoStore.todos,
      addNewTodo,
      remove,
      toggleCompletion,
      confirmRemove,
      incompleteTodosCount
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Metal+Mania&display=swap');

body {
  font-family: 'Metal Mania', cursive;
  background-color: #1c1c1c;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-image: url(https://wallpapercave.com/wp/wp2673292.jpg);
  background-repeat: no-repeat;
  background-size: cover;
}

.container {
  max-width: 600px;
  margin: 60px auto;
  padding: 20px;
  text-align: center;
  background-color: #2e2e2e;
  border-radius: 8px;
  box-shadow: 0px 0px 80px 0px rgba(0, 0, 0, 0.8);
  transition: all 0.3s ease;
  color: #f0f2f5;
}

h1 {
  color: #e60000;
  margin-bottom: 20px;
  font-size: 3em;
}

.input-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.todo-input {
  padding: 10px;
  font-size: 16px;
  border: 2px solid #555;
  border-radius: 4px;
  flex: 1;
  margin-right: 20px;
  transition: border-color 0.3s ease;
  background-color: #1c1c1c;
  color: #f0f2f5;
}

.todo-input:focus {
  border-color: #e60000;
  outline: none;
}

.add-button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #e60000;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.add-button:hover {
  background-color: #b30000;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #444;
  color: #f0f2f5;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 4px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.8);
  transition: transform 0.3s ease;
}

.todo-item:hover {
  transform: scale(1.02);
}

.todo-title {
  flex: 1;
  text-align: left;
  cursor: pointer;
  transition: color 0.3s ease;
}

.todo-title.completed {
  text-decoration: line-through;
  color: #888;
}

.delete-button {
  background-color: #e60000;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.delete-button:hover {
  background-color: #b30000;
}

</style>
