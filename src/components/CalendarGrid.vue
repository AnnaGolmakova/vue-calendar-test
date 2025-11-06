<script setup lang="ts">
import {
  CalendarDate,
  getLocalTimeZone,
  getWeeksInMonth,
  isSameDay,
  parseDate,
  startOfMonth,
  startOfWeek,
  today,
} from "@internationalized/date";
import Day from "./Day.vue";

interface Props {
  month: string | CalendarDate;
  selectedDay?: CalendarDate;
  locale?: string;
}

const { month, selectedDay, locale = "ru-RU" } = defineProps<Props>();

const dayToday = today(getLocalTimeZone());
const monthStart = startOfMonth(
  typeof month === "string" ? parseDate(month) : month,
);
const calendarGridStartDay = startOfWeek(monthStart, locale);
const weeksInMonth = getWeeksInMonth(today(getLocalTimeZone()), locale);

const calendarDays = Array.from({ length: weeksInMonth }, (_, week: number) => {
  return Array.from({ length: 7 }, (_, day: number) =>
    calendarGridStartDay.add({ weeks: week, days: day }),
  );
});
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

<style scoped></style>
