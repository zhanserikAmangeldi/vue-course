<script setup>
import { ref, computed } from 'vue'

let id = 0

// reactive vars
const newTask = ref('')
const newCategory = ref('')
const newPriority = ref('Low')
const selectedCategory = ref('All')
const selectedPriority = ref('All')
const hideCompleted = ref(false)
const tasks = ref([
  { id: id++, desc: "First Task", done: true, priority: "High", category: "General" },
  { id: id++, desc: "Second Task", done: false, priority: "Medium", category: "Work" },
  { id: id++, desc: "Third Task", done: false, priority: "Low", category: "Personal" }
])


const categories = computed(() => {
  const set = new Set(tasks.value.map(task => task.category))
  return ['All', ...set]
})

// computed vars
const incompleteTasksCount = computed(() => {
  return tasks.value.filter((task) => !task.done).length
})

const filteredTasks = computed(() => {
  let list = tasks.value

  if (hideCompleted.value) {
    list = list.filter((task) => !task.done)
  }

  if (selectedPriority.value !== 'All') {
    list = list.filter((task) => task.priority === selectedPriority.value)
  }

  if (selectedCategory.value !== 'All') {
    list = list.filter((task) => task.category === selectedCategory.value)
  }
 
  return list 
})

// functions
function addTask() {
  tasks.value.push({
    id: id++,
    desc: newTask.value,
    done: false,
    priority: newPriority.value,
    category: newCategory.value
  })
  newTask.value = ''
  newPriority.value = 'Low'
  newCategory.value = ''
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
    <input type="text" v-model="newCategory" required placeholder="Task's category">
    <select v-model="newPriority">
      <option>High</option>
      <option>Medium</option>
      <option>Low</option>
    </select>
    <button>Add the task</button>
  </form>
  <span>Completed task counts: {{ incompleteTasksCount }}</span>
  <div>
    <label>Filter by category:</label>
    <select v-model="selectedCategory">
      <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
    </select>
    <select v-model="selectedPriority">
      <option>All</option>
      <option>Low</option>
      <option>Medium</option>
      <option>High</option>
    </select>
  </div>
  <ul v-if="filteredTasks.length != 0">
    <li v-for="task in filteredTasks" :key="task.id">
      <input type="checkbox" v-model="task.done">
      <span :class="{ done: task.done}"> {{ task.desc }}, {{ task.category }} - {{ task.priority }} </span>
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
