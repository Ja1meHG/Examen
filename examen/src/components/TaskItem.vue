<template>
  <td>
    <div>{{ task.id }}</div>
    <div>{{ task.title }}</div>
    <div>{{ task.completed ? 'Terminada' : 'Activa' }}</div>
    <div>
      <input type="checkbox" v-model="task.completed" @change="completeTask">
    </div>
    <div>
      <button @click="deleteTask">Eliminar</button>
    </div>
  </td>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';

interface Task {
  id: number;
  title: string;
  completed: boolean;
}

const props = defineProps<{
  task: Task;
}>()

const emits = defineEmits(['task-completed', 'task-deleted']);

const completeTask = () => {
  emits('task-completed');
};

const deleteTask = () => {
  emits('task-deleted');
};
</script>

<style scoped>
.task-item {
  display: flex;
  flex-direction: column;
}
</style>
