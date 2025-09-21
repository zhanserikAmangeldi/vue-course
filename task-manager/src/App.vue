<script setup>
import { ref, computed } from 'vue'

let id = 0

// reactive vars
const newTask = ref('')
const hideCompleted = ref(false)
const tasks = ref([
  { id: id++, desc: "First Task", done: true },
  { id: id++, desc: "Second Task", done: false },
  { id: id++, desc: "Third Task", done: false }
])


// computed vars
const incompletedTasksCount = computed(() => {
  return tasks.value.filter((task) => !task.done).length
})

const filteredTasks = computed(() => {
  return hideCompleted.value
    ? tasks.value.filter((task) => !task.done)
    : tasks.value
})

// functions
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
  <span>Completed task counts: {{ incompletedTasksCount }}</span>
  <ul v-if="filteredTasks.length != 0">
    <li v-for="task in filteredTasks" :key="task.id">
      <input type="checkbox" v-model="task.done">
      <span :class="{ done: task.done}">{{ task.desc }}</span>
      <button @click="removeTask(task.id)">x</button>
    </li>
  </ul>
  <p v-else>There are not tasks :[</p>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed'}}
  </button>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
