<script setup>
import { defineProps } from "vue";
const props = defineProps({
  task: {
    type: Object,
  },
});
</script>
<template>
  <VCard
    :title="task.title"
    :subtitle="task.type"
    :prepend-icon="task.type === 'bug' ? 'mdi-bug' : 'mdi-shape-square-plus'"
    variant="tonal"
    color="indigo-darken-3"
    :append-icon="task.status === 'Done' ? 'mdi-check' : null"
  >
    <VCardText>{{ task.description }}</VCardText>
  </VCard>
</template>
