<script setup>
import { ref, computed } from 'vue'
import TaskForm from './components/TaskForm.vue'
import TaskList from './components/TaskList.vue'

let id = 0

// reactive vars
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
function addTask(taskData) {
  tasks.value.push({
    id: id++,
    ...taskData,
    done: false
  })
}

function removeTask(taskId) {
  tasks.value = tasks.value.filter((task) => {
    return task.id != taskId
  })
}

function updateTask(taskId, updates) {
  const taskIndex = tasks.value.findIndex(task => task.id === taskId)
  if (taskIndex !== -1) {
    tasks.value[taskIndex] = { ...tasks.value[taskIndex], ...updates }
  }
}
</script>

<template>
  <div class="app">
    <h1>Task Manager by Zhanserik</h1>

    <!-- Task Form Component -->
    <TaskForm @add-task="addTask" />

    <!-- Counter -->
    <span class="counter">Tasks remaining: {{ incompleteTasksCount }}</span>

    <!-- Filter -->
    <div class="filters">
      <label>Filter by category:</label>
      <select v-model="selectedCategory">
        <option v-for="category in categories" :key="category" :value="category">
          {{ category }}
        </option>
      </select>
      <select v-model="selectedPriority">
        <option>All</option>
        <option>Low</option>
        <option>Medium</option>
        <option>High</option>
      </select>
      
      <!-- Hide/Show -->
      <button @click="hideCompleted = !hideCompleted" class="toggle-btn">
        {{ hideCompleted ? 'Show all' : 'Hide completed' }}
      </button>
    </div>

    <!-- Task List Component -->
    <TaskList 
      :tasks="filteredTasks" 
      @remove-task="removeTask"
      @update-task="updateTask"
    />
  </div>
</template>

<style scoped>
.app {
  max-width: 600px;
  margin: 40px auto;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  font-family: 'Segoe UI', Roboto, sans-serif;
  background: #ffffff;
  color: #333;
}

h1 {
  text-align: center;
  margin-bottom: 30px;
  color: #2c3e50;
  font-weight: 300;
  font-size: 2em;
}

.counter {
  display: block;
  text-align: center;
  margin: 20px 0;
  font-size: 16px;
  color: #666;
  font-weight: 500;
}

.filters {
  display: flex;
  gap: 15px;
  margin-bottom: 25px;
  align-items: center;
  flex-wrap: wrap;
}

.filters label {
  font-weight: 500;
  color: #555;
}

.filters select {
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  background: white;
  cursor: pointer;
}

.toggle-btn {
  display: block;
  margin: 0 0 0 auto;
  padding: 10px 20px;
  background: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s ease;
  font-weight: 500;
}

.toggle-btn:hover {
  background: #e9ecef;
}
</style>