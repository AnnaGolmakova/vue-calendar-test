<script setup lang="ts">
interface Props {
  isSelected?: boolean;
  isToday?: boolean;
  isWeekend?: boolean;
  label: string;
}

const { isSelected, isToday, isWeekend, label } = defineProps<Props>();
</script>

<template>
  <button
    class="day"
    :class="{ selected: isSelected, today: isToday, weekend: isWeekend }"
  >
    {{ label }}
  </button>
</template>

<style scoped>
.day {
  display: grid;
  flex-direction: row;
  place-items: center;
  box-sizing: border-box;
  border: none;
  border-radius: 8px;
  background-color: transparent;
  aspect-ratio: 1;
  min-width: 32px;
  min-height: 32px;
  font-size: 14px;
  text-align: center;

  &:hover {
    background-color: oklch(0.94 0.015 250);
  }
}

.selected {
  background-color: oklch(0.9 0.02 250);
  font-weight: 600;

  &:hover {
    background-color: oklch(0.9 0.02 250);
  }
}

.today {
  border: solid 1px oklch(0.8 0.02 250 / 60%);
}

.weekend {
  color: red;
}
</style>
