<script setup>
import { ref } from "vue";
import Tasks from "@/data/tasks.json";

const tasks = ref(Tasks);
</script>

<template>
  <div class="grid grid-cols-3">
    <VCard v-for="task in tasks" :key="task.id">
      <VCardTitle>{{ task.title }}</VCardTitle>
      <VCardText>{{ task.status }}</VCardText>
    </VCard>
  </div>
</template>
