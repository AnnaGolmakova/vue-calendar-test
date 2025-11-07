<script setup lang="ts">
import {
  CalendarDate,
  DateFormatter,
  getLocalTimeZone,
  getWeeksInMonth,
  isSameDay,
  isSameMonth,
  isWeekend,
  startOfMonth,
  startOfWeek,
  today,
} from "@internationalized/date";
import { computed } from "vue";
import Day from "./Day.vue";

interface Props {
  month: CalendarDate;
  selectedDay?: CalendarDate;
  showDatesOutOfMonth?: boolean;
  showWeekdays?: boolean;
  highlightwWeekends?: boolean;
  locale?: string;
}

const {
  month,
  selectedDay,
  showDatesOutOfMonth = false,
  showWeekdays = true,
  highlightwWeekends = true,
  locale = "ru-RU",
} = defineProps<Props>();

const dayToday = today(getLocalTimeZone());
const monthStart = computed(() => startOfMonth(month));

const calendarGridStartDay = computed(() =>
  startOfWeek(monthStart.value, locale),
);

const weekdayFormatter = computed(
  () => new DateFormatter(locale, { weekday: "short" }),
);

const weekdays = computed(() =>
  Array.from({ length: 7 }, (_, i: number) => {
    const date = calendarGridStartDay.value.add({ days: i });
    const dateDay = date.toDate(getLocalTimeZone());
    return weekdayFormatter.value.format(dateDay);
  }),
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
      <tr v-if="showWeekdays">
        <td v-for="label in weekdays" class="weekday">
          {{ label }}
        </td>
      </tr>
      <tr v-for="weeks in calendarDays">
        <td v-for="day in weeks">
          <Day
            v-if="showDatesOutOfMonth || isSameMonth(day, monthStart)"
            :label="day.day.toString()"
            :isToday="isSameDay(day, dayToday)"
            :isSelected="selectedDay && isSameDay(day, selectedDay)"
            :isWeekend="highlightwWeekends && isWeekend(day, locale)"
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

.weekday {
  color: rgba(0, 0, 0, 0.6);
  font-size: 12px;
  text-align: center;
}
</style>
