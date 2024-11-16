<template>
  <div class="app">
    <h1>Animated To-Do List by <br> 21B030728 a_faizolla@kbtu.kz</h1>

    <AddTaskForm @add-task="addTask" />

    <p class="pending-tasks">Pending Tasks: {{ pendingTasks }}</p>

    <TaskList :tasks="tasks" @delete-task="deleteTask" @update-task="updateTask" />
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch, nextTick } from 'vue';
import { Task } from './types/Task';
import AddTaskForm from './components/AddTaskForm.vue';
import TaskList from './components/TaskList.vue';

const tasks = ref<Task[]>([]);

const addTask = async (task: Task) => {
  tasks.value.push(task);
  console.log('Task added:', task);

  await nextTick();
  scrollToTask(task.id);
};

const deleteTask = (id: number) => {
  tasks.value = tasks.value.filter((task) => task.id !== id);
  console.log('Task deleted:', id);
};

const updateTask = (updatedTask: Task) => {
  const index = tasks.value.findIndex((task) => task.id === updatedTask.id);
  if (index !== -1) {
    tasks.value[index] = { ...updatedTask };
    console.log('Task updated:', updatedTask);
  }
};

const pendingTasks = computed(() => tasks.value.filter((task) => !task.completed).length);

watch(
  tasks,
  (newVal, oldVal) => {
    if (newVal.length > oldVal.length) {
      console.log('A task was added.');
    }
  },
  { deep: true }
);

const scrollToTask = (id: number) => {
  const element = document.getElementById(`task-${id}`);
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' });
  }
};
</script>

<style scoped>
.app {
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  font-family: Arial, sans-serif;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
}

.pending-ttasks {
  text-align: center;
  font-size: 1.2rem;
  color: #555;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateY(20px);
}
</style>