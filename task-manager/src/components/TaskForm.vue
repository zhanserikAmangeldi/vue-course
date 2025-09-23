<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add-task'])

const newTask = ref('')
const newCategory = ref('')
const newPriority = ref('Low')

function addTask() {
  if (newTask.value.trim() && newCategory.value.trim()) {
    emit('add-task', {
      desc: newTask.value,
      category: newCategory.value,
      priority: newPriority.value
    })
    
    // Reset form
    newTask.value = ''
    newCategory.value = ''
    newPriority.value = 'Low'
  }
}
</script>

<template>
  <form class="task-form" @submit.prevent="addTask">
    <input 
      type="text" 
      v-model="newTask" 
      required 
      placeholder="New Task" 
    />
    <input 
      type="text" 
      v-model="newCategory" 
      required 
      placeholder="Task's category" 
    />
    <select v-model="newPriority">
      <option>High</option>
      <option>Medium</option>
      <option>Low</option>
    </select>
    <button type="submit" class="btn-add">Add</button>
  </form>
</template>

<style scoped>
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
</style>