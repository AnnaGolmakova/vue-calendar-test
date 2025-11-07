<script setup lang="ts">
import {
  getLocalTimeZone,
  parseDate,
  startOfMonth,
  today,
} from "@internationalized/date";
import { ref } from "vue";
import CalendarGrid from "./CalendarGrid.vue";
import MonthSwitch from "./MonthSwitch.vue";

interface Props {
  selectedDay?: string | Date;
  locale?: string;
}

const { selectedDay, locale = "ru-RU" } = defineProps<Props>();

const currentSelection = selectedDay
  ? convertToCalendarDate(selectedDay)
  : undefined;

const currentMonth = ref(
  startOfMonth(currentSelection ? currentSelection : today(getLocalTimeZone())),
);

function convertToCalendarDate(date: string | Date) {
  if (typeof date === "string") {
    return parseDate(date);
  } else {
    return parseDate(date.toISOString());
  }
}

function showNextMonth() {
  currentMonth.value = currentMonth.value.add({ months: 1 });
}

function showPreviousMonth() {
  currentMonth.value = currentMonth.value.subtract({ months: 1 });
}
</script>

<template>
  <div class="container">
    <MonthSwitch
      :date="currentMonth"
      :locale="locale"
      @on-back="showPreviousMonth"
      @on-next="showNextMonth"
    />
    <CalendarGrid :month="currentMonth" :locale="locale" />
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex: none;
  flex-direction: column;
  gap: 8px;
  box-shadow: 0 4px 16px -4px rgba(0, 0, 0, 0.12);
  border: solid 1px rgba(0, 0, 0, 0.06);
  border-radius: 16px;
  background: white;
  padding: 8px;
  max-width: 320px;
}
</style>
