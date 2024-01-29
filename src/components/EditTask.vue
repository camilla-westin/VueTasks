<script setup>
import { ref, defineProps, watch } from "vue";

const props = defineProps({
  dialogStatus: {
    type: Boolean,
  },
});

const dialog = ref(false);
const taskTitle = ref("");
const taskDescription = ref("");
const taskStatus = ref("");
const taskType = ref("");

const emit = defineEmits(["update:dialogStatus", "onSaved"]);

const onSave = () => {
  const taskData = {
    title: taskTitle.value,
    description: taskDescription.value,
    status: taskStatus.value,
    type: taskType.value,
  };

  emit("onSaved", taskData);

  dialog.value = false;
  emit("update:dialogStatus", false);
};

watch(
  () => props.dialogStatus,
  (newValue) => {
    if (newValue === true) {
      dialog.value = true;
    } else {
      dialog.value = false;
    }
  }
);

const closeDialog = () => {
  dialog.value = false;
  emit("update:dialogStatus", false);
};
</script>

<template>
  <VRow justify="center">
    <VDialog v-model="dialog" persistent width="540">
      <VCard>
        <VCardTitle>
          <span class="text-h5">Edit task</span>
        </VCardTitle>
        <VCardText>
          <VContainer>
            <VRow>
              <VCol cols="12">
                <v-text-field
                  v-model="taskTitle"
                  label="Title"
                  required
                ></v-text-field>
              </VCol>
              <VCol cols="12">
                <v-textarea
                  v-model="taskDescription"
                  label="Description"
                ></v-textarea>
              </VCol>
              <VCol cols="12" sm="6">
                <v-select
                  v-model="taskStatus"
                  :items="['To do', 'In Progress', 'Done']"
                  label="Progress"
                  required
                ></v-select>
              </VCol>
              <VCol cols="12" sm="6">
                <v-select
                  v-model="taskType"
                  :items="['Bug', 'Feature']"
                  label="Type"
                  required
                ></v-select>
              </VCol>
            </VRow>
          </VContainer>
        </VCardText>
        <VCardActions>
          <v-spacer></v-spacer>
          <VBtn color="blue-darken-1" variant="text" @click="closeDialog"
            >Close</VBtn
          >
          <VBtn color="blue-darken-1" variant="text" @click="onSave">Save</VBtn>
        </VCardActions>
      </VCard>
    </VDialog>
  </VRow>
</template>
