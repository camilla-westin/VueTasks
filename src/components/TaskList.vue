<script setup>
import { ref } from "vue";
import Tasks from "@/data/tasks.json";
import TaskCard from "@/components/TaskCard.vue";
import EditTask from "./EditTask.vue";

const tasks = ref(Tasks);
const dialogStatus = ref(false);

const deleteTask = (id) => {
  tasks.value = tasks.value.filter((task) => task.id !== id);
};

const openDialog = () => {
  dialogStatus.value = true;
};

const closeDialog = () => {
  dialogStatus.value = false;
};
</script>

<template>
  <div class="flex justify-end items-center mb-6">
    <VBtn @click="openDialog" prepend-icon="mdi-plus">Add task</VBtn>
  </div>

  <ul class="grid grid-cols-3 gap-4">
    <li v-for="task in tasks" :key="task.id">
      <TaskCard :task="task" @delete-clicked="deleteTask" />
    </li>
  </ul>

  <EditTask :dialog-status="dialogStatus" @update:dialog-status="closeDialog" />
</template>
