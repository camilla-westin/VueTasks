<script setup>
import { defineProps } from "vue";
const props = defineProps({
  task: {
    type: Object,
  },
});

const emit = defineEmits(["deleteClicked", "editClicked"]);

const deleteClicked = (id) => {
  emit("deleteClicked", id);
};

const editClicked = (id) => {
  emit("editClicked", id);
};
</script>
<template>
  <VCard
    :title="task.title"
    :subtitle="task.type"
    :prepend-icon="task.type === 'Bug' ? 'mdi-bug' : 'mdi-shape-square-plus'"
    variant="tonal"
    color="blue-darken-3"
    :append-icon="task.status === 'Done' ? 'mdi-check' : null"
    hover
  >
    <VCardText>{{ task.description }}</VCardText>
    <VDivider
      thickness="1"
      color="blue-darken-4"
      class="border-opacity-100"
    ></VDivider>

    <VCardActions>
      <VBtn
        @click="editClicked(task.id)"
        prepend-icon="mdi-pencil"
        color="blue-darken-2"
        outlined
      >
        Edit
      </VBtn>
      <VBtn
        @click="deleteClicked(task.id)"
        prepend-icon="mdi-delete"
        color="blue-darken-2"
        outlined
      >
        Delete
      </VBtn>
    </VCardActions>
  </VCard>
</template>
