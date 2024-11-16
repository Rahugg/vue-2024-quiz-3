<template>
    <transition-group name="list" tag="ul" class="task-list">
        <TaskItem v-for="task in sortedTasks" :key="task.id" :task="task" @delete-task="onDeleteTask"
            @update-task="onUpdateTask" />
    </transition-group>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { Task } from '../types/Task';
import TaskItem from './TaskItem.vue';

const props = defineProps<{
    tasks: Task[];
}>();

const emit = defineEmits<{
    (e: 'delete-task', id: number): void;
    (e: 'update-task', task: Task): void;
}>();

const sortedTasks = computed(() => {
    const priorityOrder = { High: 0, Medium: 1, Low: 2 };
    return [...props.tasks].sort(
        (a, b) => priorityOrder[a.priority] - priorityOrder[b.priority]
    );
});

const onDeleteTask = (id: number) => {
    emit('delete-task', id);
};

const onUpdateTask = (task: Task) => {
    emit('update-task', task);
};
</script>

<style scoped>
.task-list {
    list-style: none;
    padding: 0;
    margin: 0;
}

.list-enter-active,
.list-leave-active {
    transition: all 0.5s;
}

.list-enter,
.list-leave-to {
    opacity: 0;
    transform: translateY(30px);
}
</style>