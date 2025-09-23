<script setup>
import TaskItem from './TaskItem.vue'

defineProps({
  tasks: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['remove-task', 'update-task'])

function handleRemoveTask(taskId) {
  emit('remove-task', taskId)
}

function handleUpdateTask(taskId, updates) {
  emit('update-task', taskId, updates)
}
</script>

<template>
  <ul v-if="tasks.length > 0" class="task-list">
    <TaskItem 
      v-for="task in tasks" 
      :key="task.id" 
      :task="task"
      @remove-task="handleRemoveTask"
      @update-task="handleUpdateTask"
    />
  </ul>
  <p v-else class="no-tasks">There are no tasks :[</p>
</template>

<style scoped>
.task-list {
  list-style: none;
  padding: 0;
  margin: 0 0 25px 0;
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
</style>