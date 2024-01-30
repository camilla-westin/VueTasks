<script setup>
import { ref, computed } from "vue";
import Tasks from "@/data/tasks.json";
import TaskCard from "@/components/TaskCard.vue";
import EditTask from "./EditTask.vue";

const tasks = ref(Tasks);
const dialogStatus = ref(false);
const editTaskData = ref(null);
const showAllTasks = ref(true);
const showBugs = ref(false);
const showFeatures = ref(false);

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

const handleCheckboxChange = (type) => {
  if (type === "All") {
    showAllTasks.value = true;
    showBugs.value = false;
    showFeatures.value = false;
  } else if (type === "Bug") {
    showAllTasks.value = false;
    showBugs.value = true;
    showFeatures.value = false;
  } else if (type === "Feature") {
    showAllTasks.value = false;
    showBugs.value = false;
    showFeatures.value = true;
  }
};

const selectedFilterType = computed(() => {
  if (showAllTasks.value) {
    return "All";
  } else if (showBugs.value) {
    return "Bug";
  } else if (showFeatures.value) {
    return "Feature";
  }
});

const filteredTasks = computed(() =>
  tasks.value.filter((task) => {
    const filterType = selectedFilterType.value;
    return filterType === "All" || task.type === filterType;
  })
);
</script>

<template>
  <div class="flex justify-end items-center mb-6">
    <VForm class="mr-6 flex items-center">
      <VLabel>Filter by type</VLabel>
      <VCheckboxBtn
        v-model="showAllTasks"
        label="All"
        class="text-sm ml-3"
        color="indigo"
        @change="() => handleCheckboxChange('All')"
        inline
      ></VCheckboxBtn>
      <VCheckboxBtn
        v-model="showBugs"
        label="Bug"
        class="text-sm"
        color="indigo"
        @change="() => handleCheckboxChange('Bug')"
        inline
      ></VCheckboxBtn>
      <VCheckboxBtn
        v-model="showFeatures"
        label="Feature"
        class="text-sm"
        @change="() => handleCheckboxChange('Feature')"
        color="indigo"
      ></VCheckboxBtn>
    </VForm>
    <VBtn @click="openDialog" prepend-icon="mdi-plus">Add task</VBtn>
  </div>
  <VRow class="grid grid-cols-3 gap-4">
    <VCol class="p-4 border border-gray-600 rounded shadow-lg">
      <h2 class="font-medium text-md mb-4 uppercase">To do</h2>

      <ul>
        <li
          v-for="task in filteredTasks.filter(
            (task) => task.status === 'To do'
          )"
          :key="task.id"
          class="mb-4"
        >
          <TaskCard
            :task="task"
            @delete-clicked="deleteTask"
            @editClicked="editTask"
          />
        </li>
      </ul>
    </VCol>
    <VCol class="p-4 border border-gray-600 rounded shadow-lg">
      <h2 class="font-medium text-md mb-4 uppercase">In Progress</h2>
      <VDivider></VDivider>
      <ul>
        <li
          v-for="task in filteredTasks.filter(
            (task) => task.status === 'In progress'
          )"
          :key="task.id"
          class="mb-4"
        >
          <TaskCard
            :task="task"
            @delete-clicked="deleteTask"
            @editClicked="editTask"
          />
        </li>
      </ul>
    </VCol>
    <VCol class="p-4 border border-gray-600 rounded shadow-lg">
      <h2 class="font-medium text-md mb-4 uppercase">Done</h2>

      <ul>
        <li
          v-for="task in filteredTasks.filter((task) => task.status === 'Done')"
          :key="task.id"
          class="mb-4"
        >
          <TaskCard
            :task="task"
            @delete-clicked="deleteTask"
            @editClicked="editTask"
          />
        </li>
      </ul>
    </VCol>
  </VRow>

  <EditTask
    :dialog-status="dialogStatus"
    :edit-task-data="editTaskData"
    @update:dialog-status="closeDialog"
    @on-saved="submitTask"
  />
</template>
