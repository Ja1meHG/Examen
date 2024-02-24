<template>
  <div class="TareasPen">
    <slot name="header"></slot>
    <div v-if="tasks.length === 0">No hay tareas</div>
    <table v-else>
      <thead>
        <tr>
          <th>Número de Tarea</th>
          <th>Tarea</th>
          <th>Materia</th>
          <th>Estado</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="task.id">
          <td>{{ task.id }}</td>
          <td>{{ task.title }}</td>
          <td>{{ task.subject }}</td>
          <td>{{ task.completed ? 'Terminada' : 'Activa' }}</td>
          <td>
            <button @click="completeTask(index)" v-if="!task.completed">Completar</button>
            <button @click="deleteTask(index)">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
    <div>
      <input v-model.trim="newTask.title" type="text" placeholder="Nueva tarea">
      <input v-model.trim="newTask.subject" type="text" placeholder="Materia">
      <button @click="addTask">Agregar tarea</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import TaskItem from './TaskItem.vue';

interface Task {
  id: number;
  title: string;
  subject: string;
  completed: boolean;
}

const tasks = ref<Task[]>([
  { id: 1, title: '1000 ejercicios de antiderivada', subject: 'Matemáticas', completed: false },
  { id: 2, title: 'Maqueta de una molecula', subject: 'Ciencias', completed: true }
]);

const addTask = () => {
  if (!newTask.value.title.trim() || !newTask.value.subject.trim()) {
    alert('Por favor, complete tanto la tarea como la materia.');
    return;
  }

  const task: Task = {
    id: tasks.value.length + 1,
    title: newTask.value.title,
    subject: newTask.value.subject,
    completed: false
  };
  tasks.value.push(task);
  newTask.value = { title: '', subject: '' };
};

const completeTask = (index: number) => {
  tasks.value[index].completed = true;
};

const deleteTask = (index: number) => {
  tasks.value.splice(index, 1);
};

const newTask = ref<{ title: string; subject: string }>({ title: '', subject: '' });

const totalTasks = computed(() => tasks.value.length);
const completedTasks = computed(() => tasks.value.filter(task => task.completed).length);
</script>

<style scoped>
.TareasPen {
  color: black;
}

.TareasPen table {
  width: 100%;
  border-collapse: collapse;
}

.TareasPen th, .TareasPen td {
  border: 1px solid #ccc;
  padding: 8px;
}

.TareasPen th {
  background-color: #ff37f5;
}

.TareasPen button {
  background-color: #d000ff;
  color: rgb(0, 0, 0);
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}

.TareasPen button:hover {
  background-color: #a000ff;
}

.TareasPen input {
  margin-right: 10px;
}
</style>
