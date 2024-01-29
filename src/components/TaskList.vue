<script setup>
import { ref } from "vue";
import Tasks from "@/data/tasks.json";
import TaskCard from "@/components/TaskCard.vue";

const tasks = ref(Tasks);

const deleteTask = (id) => {
  tasks.value = tasks.value.filter((task) => task.id !== id);
};
</script>

<template>
  <ul class="grid grid-cols-3 gap-4">
    <li v-for="task in tasks" :key="task.id">
      <TaskCard :task="task" @delete-clicked="deleteTask" />
    </li>
  </ul>
</template>
