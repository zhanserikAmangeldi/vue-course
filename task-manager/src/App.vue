<script setup>
import { ref } from 'vue'

let id = 0
const newTask = ref('')
const tasks = ref([
  { id: id++, desc: "First Task"},
  { id: id++, desc: "Second Task"},
  { id: id++, desc: "Third Task"}
])

function addTask() {
  tasks.value.push({
    id: id++,
    desc: newTask.value
  })
  newTask.value = ''
}

function removeTask(id) {
  tasks.value = tasks.value.filter((task) => {
    return task.id != id
  })
}

</script>

<template>
  <form @submit.prevent="addTask">
    <input type="text" v-model="newTask" required placeholder="New Task">
    <button>Add the task</button>
  </form>
  <ul>
    <li v-for="task in tasks" :key="task.id">
      {{ task.desc }}
      <button @click="removeTask(task.id)">x</button>
    </li>
  </ul>

</template>

<style scoped></style>
