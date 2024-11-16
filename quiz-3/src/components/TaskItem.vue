<template>
    <li :id="'task-' + task.id" :class="[{ completed: task.completed }, task.priority.toLowerCase()]" class="task-item">
        <input type="checkbox" v-model="task.completed" @change="onStatusChange" class="task-checkbox" />
        <span contenteditable="true" @blur="updateTaskTitle" @input="onTaskTitleInput" class="task-title">
            {{ task.title }}
        </span>
        <button @click="onDelete" class="task-delete-button">Delete</button>
    </li>
</template>

<script setup lang="ts">
import { Task } from '../types/Task';

const props = defineProps<{
    task: Task;
}>();

const emit = defineEmits<{
    (e: 'delete-task', id: number): void;
    (e: 'update-task', task: Task): void;
}>();

const onDelete = () => {
    emit('delete-task', props.task.id);
};

const onStatusChange = () => {
    emit('update-task', props.task);
};

const updateTaskTitle = (event: Event) => {
    const element = event.target as HTMLElement;
    props.task.title = element.innerText;
    emit('update-task', props.task);
};

const onTaskTitleInput = (event: Event) => {
    const element = event.target as HTMLElement;
    props.task.title = element.innerText;
};
</script>

<style scoped>
.task-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.5rem;
    margin: 0.5rem 0;
    border: 1px solid #ccc;
    border-radius: 4px;
    background-color: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease;
}

.task-item:hover {
    background-color: #f9f9f9;
}

.task-checkbox {
    margin-right: 1rem;
}

.task-title {
    flex-grow: 1;
    margin-right: 1rem;
    padding: 0.5rem;
    border: none;
    outline: none;
    font-size: 1rem;
}

.task-title[contenteditable="true"]:focus {
    border: 1px solid #007bff;
    border-radius: 4px;
    background-color: #eef;
}

.task-delete-button {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 4px;
    background-color: #dc3545;
    color: white;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.task-delete-button:hover {
    background-color: #c82333;
}

li.completed .task-title {
    text-decoration: line-through;
    color: #888;
}

li.high {
    border-left: 5px solid red;
}

li.medium {
    border-left: 5px solid orange;
}

li.low {
    border-left: 5px solid green;
}
</style>