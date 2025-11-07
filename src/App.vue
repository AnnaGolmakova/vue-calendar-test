<script setup lang="ts">
import Calendar from "./components/Calendar.vue";
</script>

<template>
  <main class="layout">
    <h1>Календарь</h1>
    <Calendar locale="pt-PT" @change="console.log" />
  </main>
</template>

<style scoped>
.layout {
  display: flex;
  flex-direction: column;
  align-items: left;
  min-height: 400px;
}
</style>
