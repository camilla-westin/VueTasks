<script setup>
import { ref } from "vue";
import Tasks from "@/data/tasks.json";
import TaskCard from "@/components/TaskCard.vue";
import EditTask from "./EditTask.vue";

const tasks = ref(Tasks);
const dialogStatus = ref(false);
const editTaskData = ref(null);

const deleteTask = (id) => {
  tasks.value = tasks.value.filter((task) => task.id !== id);
};

const openDialog = () => {
  dialogStatus.value = true;
};

const closeDialog = () => {
  dialogStatus.value = false;
};

const submitTask = (taskData) => {
  const taskExists = tasks.value.find((task) => task.id === taskData.id);

  if (taskExists) {
    tasks.value = tasks.value.map((task) => {
      if (task.id === taskData.id) {
        return {
          ...task,
          ...taskData,
        };
      }
      return task;
    });
  } else {
    tasks.value.push({
      id: Math.floor(Math.random() * 1000000),
      ...taskData,
    });
  }

  closeDialog();
};

const editTask = (id) => {
  const currentTask = tasks.value.find((task) => task.id === id);

  editTaskData.value = {
    id: currentTask.id,
    title: currentTask.title,
    description: currentTask.description,
    status: currentTask.status,
    type: currentTask.type,
  };

  openDialog();
};
</script>

<template>
  <div class="flex justify-end items-center mb-6">
    <VBtn @click="openDialog" prepend-icon="mdi-plus">Add task</VBtn>
  </div>
  <VRow class="grid grid-cols-3 gap-4">
    <VCol>
      <ul>
        <li v-for="task in tasks" :key="task.id" class="mb-4">
          <TaskCard
            :task="task"
            @delete-clicked="deleteTask"
            @editClicked="editTask"
          />
        </li>
      </ul>
    </VCol>
    <VCol></VCol>
    <VCol></VCol>
  </VRow>

  <EditTask
    :dialog-status="dialogStatus"
    :edit-task-data="editTaskData"
    @update:dialog-status="closeDialog"
    @on-saved="submitTask"
  />
</template>
