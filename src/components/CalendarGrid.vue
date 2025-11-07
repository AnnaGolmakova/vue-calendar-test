<script setup lang="ts">
import {
  CalendarDate,
  getLocalTimeZone,
  getWeeksInMonth,
  isSameDay,
  startOfMonth,
  startOfWeek,
  today,
} from "@internationalized/date";
import { computed } from "vue";
import Day from "./Day.vue";

interface Props {
  month: CalendarDate;
  selectedDay?: CalendarDate;
  locale?: string;
}

const { month, selectedDay, locale = "ru-RU" } = defineProps<Props>();

const dayToday = today(getLocalTimeZone());

const monthStart = computed(() => startOfMonth(month));
const calendarGridStartDay = computed(() =>
  startOfWeek(monthStart.value, locale),
);

const weeksInMonth = computed(() =>
  getWeeksInMonth(today(getLocalTimeZone()), locale),
);

const calendarDays = computed(() =>
  Array.from({ length: weeksInMonth.value }, (_, week: number) => {
    return Array.from({ length: 7 }, (_, day: number) =>
      calendarGridStartDay.value.add({ weeks: week, days: day }),
    );
  }),
);
</script>

<template>
  <table>
    <tbody>
      <tr v-for="weeks in calendarDays">
        <td v-for="day in weeks">
          <Day
            :label="day.day.toString()"
            :isToday="isSameDay(day, dayToday)"
            :isSelected="selectedDay && isSameDay(day, selectedDay)"
          />
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
table {
  border-collapse: collapse;
}
td {
  padding: 0;
}
</style>
