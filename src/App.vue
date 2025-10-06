<template>
  <div class="app-container">
    <h1>Gestor de Tareas</h1>

    
    <div class="input-area">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Escribe una nueva tarea"
      />
      <button @click="addTask">Agregar</button>
    </div>

   
    <div v-if="tasks.todo.length || tasks.doing.length || tasks.done.length" class="task-sections">
      <div class="section todo">
        <h2>To Do</h2>
        <p v-if="!tasks.todo.length">Sin tareas pendientes</p>
        <ul>
          <li v-for="(task, index) in tasks.todo" :key="'todo-' + index">
            {{ task }}
            <div class="buttons">
              <button class="move" @click="moveTask('todo', 'doing', index)">➡️</button>
            </div>
          </li>
        </ul>
      </div>

      <div class="section doing">
        <h2>Doing</h2>
        <p v-if="!tasks.doing.length">Nada en progreso</p>
        <ul>
          <li v-for="(task, index) in tasks.doing" :key="'doing-' + index">
            {{ task }}
            <div class="buttons">
              <button class="move" @click="moveTask('doing', 'todo', index)">⬅️</button>
              <button class="move" @click="moveTask('doing', 'done', index)">➡️</button>
            </div>
          </li>
        </ul>
      </div>

      <div class="section done">
        <h2>Done ✅</h2>
        <p v-if="!tasks.done.length">Aún no hay tareas completadas</p>
        <ul>
          <li v-for="(task, index) in tasks.done" :key="'done-' + index">
            {{ task }}
            <span class="locked">✔️</span>
          </li>
        </ul>
      </div>
    </div>

    
    <div v-else>
      <p class="empty-message">No hay tareas registradas.</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'


const newTask = ref('')
const tasks = ref({
  todo: [],
  doing: [],
  done: [],
})


const addTask = () => {
  const name = newTask.value.trim()
  if (name) {
    tasks.value.todo.push(name)
    newTask.value = ''
  }
}


const moveTask = (from, to, index) => {
  const task = tasks.value[from][index]
  tasks.value[from].splice(index, 1)
  if (to !== 'done' || !tasks.value.done.includes(task)) {
    tasks.value[to].push(task)
  }
}
</script>

<style>
.app-container {
  max-width: 900px;
  margin: 40px auto;
  font-family: "Poppins", sans-serif;
  text-align: center;
}

h1 {
  color: #fd0d0d;
  margin-bottom: 20px;
}

.input-area {
  margin-bottom: 30px;
}

input {
  padding: 10px;
  font-size: 16px;
  width: 60%;
  border: 2px solid #fc120ac3;
  border-radius: 8px;
}

button {
  margin-left: 8px;
  padding: 10px 15px;
  background-color: #e00404c6;
  color: rgb(19, 1, 1);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background-color: #81bddf;
}

.task-sections {
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

.section {
  flex: 1;
  background-color: #f3e5f5;
  border-radius: 12px;
  padding: 15px;
  box-shadow: 0 2px 8px rgba(247, 243, 243, 0.9);
}

.section h2 {
  background-color: #ff0707;
  color: rgb(242, 240, 240);
  border-radius: 8px;
  padding: 8px;
  margin-bottom: 10px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background-color: white;
  border-radius: 8px;
  padding: 10px;
  margin: 8px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.buttons button {
  background-color: #fb0707;
  margin-left: 5px;
}

.locked {
  color: green;
  font-weight: bold;
}

.empty-message {
  color: rgb(22, 1, 1);
  font-style: italic;
}
</style>
