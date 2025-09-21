<script setup>
import { ref, computed } from 'vue'

let id = 0

const newTask = ref('')
const hideCompleted = ref(false)
const tasks = ref([
  { id: id++, desc: "First Task", done: true },
  { id: id++, desc: "Second Task", done: false },
  { id: id++, desc: "Third Task", done: false }
])

const completedTasks = computed(() => {
  return hideCompleted.value
    ? tasks.value.filter((task) => !task.done)
    : tasks.value
})

function addTask() {
  tasks.value.push({
    id: id++,
    desc: newTask.value,
    done: false
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
    <li v-for="task in completedTasks" :key="task.id">
      <input type="checkbox" v-model="task.done">
      <span :class="{ done: task.done}">{{ task.desc }}</span>
      <button @click="removeTask(task.id)">x</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed'}}
  </button>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
