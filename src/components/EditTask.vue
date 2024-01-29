<script setup>
import { ref, defineProps, watch } from "vue";

const props = defineProps({
  dialogStatus: {
    type: Boolean,
  },
});

const dialog = ref(false);

const emit = defineEmits(["update:dialogStatus"]);

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
                <v-text-field label="Task name" required></v-text-field>
              </VCol>
              <VCol cols="12">
                <v-textarea label="Description"></v-textarea>
              </VCol>
              <VCol cols="12" sm="6">
                <v-select
                  :items="['To do', 'In Progress', 'Done']"
                  label="Progress"
                  required
                ></v-select>
              </VCol>
              <VCol cols="12" sm="6">
                <v-select
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
          <v-btn color="blue-darken-1" variant="text" @click="closeDialog">
            Close
          </v-btn>
          <v-btn color="blue-darken-1" variant="text" @click="dialog = false">
            Save
          </v-btn>
        </VCardActions>
      </VCard>
    </VDialog>
  </VRow>
</template>
