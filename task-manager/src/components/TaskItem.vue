<script setup>
import { computed } from 'vue'

const props = defineProps({
  task: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['remove-task', 'update-task'])

const taskDone = computed({
  get() {
    return props.task.done
  },
  set(value) {
    emit('update-task', props.task.id, { done: value })
  }
})

function removeTask() {
  emit('remove-task', props.task.id)
}
</script>

<template>
  <li class="task-item">
    <input 
      type="checkbox" 
      v-model="taskDone" 
    />
    <span :class="{ done: task.done }" class="task-content">
      {{ task.desc }}
    </span>
    <span class="task-meta">
      <span class="category">{{ task.category }}</span>
      <span class="priority" :class="task.priority.toLowerCase()">{{ task.priority }}</span>
    </span>
    <button @click="removeTask" class="btn-remove">×</button>
  </li>
</template>

<style scoped>
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
</style>