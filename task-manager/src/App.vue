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
  <div class="app">
    <h1>Task Manager by Zhanserik</h1>

    <!-- Form -->
    <form class="task-form" @submit.prevent="addTask">
      <input type="text" v-model="newTask" required placeholder="New Task" />
      <input type="text" v-model="newCategory" required placeholder="Task's category" />
      <select v-model="newPriority">
        <option>High</option>
        <option>Medium</option>
        <option>Low</option>
      </select>
      <button class="btn-add">Add</button>
    </form>

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

    <!-- List -->
    <ul v-if="filteredTasks.length != 0" class="task-list">
      <li v-for="task in filteredTasks" :key="task.id" class="task-item">
        <input type="checkbox" v-model="task.done" />
        <span :class="{ done: task.done }" class="task-content">
          {{ task.desc }}
        </span>
        <span class="task-meta">
          <span class="category">{{ task.category }}</span>
          <span class="priority" :class="task.priority.toLowerCase()">{{ task.priority }}</span>
        </span>
        <button @click="removeTask(task.id)" class="btn-remove">×</button>
      </li>
    </ul>
    <p v-else class="no-tasks">There are no tasks :[</p>
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

.task-form {
  display: flex;
  gap: 10px;
  margin-bottom: 25px;
  flex-wrap: wrap;
}

.task-form input,
.task-form select {
  padding: 12px 16px;
  border: 2px solid #e1e8ed;
  border-radius: 8px;
  font-size: 14px;
  flex: 1;
  min-width: 120px;
  transition: border-color 0.2s ease;
}

.task-form input:focus,
.task-form select:focus {
  outline: none;
  border-color: #3498db;
}

.btn-add {
  padding: 12px 20px;
  background: #27ae60;
  color: white;
  border: none;
  font-weight: 500;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.2s ease;
}

.btn-add:hover {
  background: #219a52;
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

.task-list {
  list-style: none;
  padding: 0;
  margin: 0 0 25px 0;
}

.task-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 16px;
  margin-bottom: 8px;
  background: #f8f9fa;
  border-radius: 8px;
  border-left: 4px solid transparent;
  transition: all 0.2s ease;
}

.task-item:hover {
  background: #f1f3f4;
  transform: translateY(-1px);
}

.task-item input[type="checkbox"] {
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.task-content {
  flex: 1;
  font-size: 16px;
  line-height: 1.4;
}

.task-meta {
  display: flex;
  gap: 8px;
  align-items: center;
}

.category {
  background: #e9ecef;
  color: #495057;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
}

.priority {
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
  text-transform: uppercase;
}

.priority.high {
  background: #fee;
  color: #dc2626;
}

.priority.medium {
  background: #fef3c7;
  color: #d97706;
}

.priority.low {
  background: #dcfce7;
  color: #16a34a;
}

.btn-remove {
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 50%;
  width: 28px;
  height: 28px;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.btn-remove:hover {
  background: #c0392b;
}

.done {
  text-decoration: line-through;
  color: #888;
}

.no-tasks {
  text-align: center;
  color: #666;
  font-style: italic;
  padding: 40px 20px;
  background: #f8f9fa;
  border-radius: 8px;
  margin-bottom: 25px;
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
