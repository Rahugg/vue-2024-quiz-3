<template>
    <form @submit.prevent="onSubmit" class="task-form">
        <input v-model="newTaskTitle" placeholder="Task Title" required class="task-input" />
        <select v-model="newTaskPriority" required class="task-select">
            <option disabled value="">Select Priority</option>
            <option value="Low">Low</option>
            <option value="Medium">Medium</option>
            <option value="High">High</option>
        </select>
        <button type="submit" class="task-button">Add Task</button>
    </form>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { Task } from '../types/Task';

const emit = defineEmits<{
    (e: 'add-task', task: Task): void;
}>();

const newTaskTitle = ref('');
const newTaskPriority = ref<Task['priority'] | ''>('');

const onSubmit = () => {
    if (!newTaskTitle.value || !newTaskPriority.value) return;

    const newTask: Task = {
        id: Date.now(),
        title: newTaskTitle.value,
        priority: newTaskPriority.value as Task['priority'],
        completed: false,
    };

    emit('add-task', newTask);

    newTaskTitle.value = '';
    newTaskPriority.value = '';
};
</script>

<style scoped>
.task-form {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    max-width: 400px;
    margin: 0 auto;
    padding: 2rem;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #f9f9f9;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.task-input,
.task-select {
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 1rem;
}

.task-button {
    padding: 0.75rem;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: white;
    font-size: 1rem;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.task-button:hover {
    background-color: #0056b3;
}
</style>