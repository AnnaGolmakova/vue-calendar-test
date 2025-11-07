<script setup lang="ts">
import {
  CalendarDate,
  DateFormatter,
  getLocalTimeZone,
} from "@internationalized/date";
import { ChevronLeft, ChevronRight } from "lucide-vue-next";
import { computed } from "vue";

interface Props {
  date: CalendarDate;
  locale?: string;
}

const { date, locale = "ru-RU" } = defineProps<Props>();

const formatter = computed(() => new DateFormatter(locale, { month: "long" }));

const formatTitle = (date: CalendarDate) => {
  let month = formatter.value.format(date.toDate(getLocalTimeZone()));
  month = month.charAt(0).toUpperCase() + month.slice(1);
  return month + " " + date.year;
};
</script>

<template>
  <div class="switcher">
    <button
      class="button"
      aria-label="Предыдущий месяц"
      @click="$emit('onBack')"
    >
      <ChevronLeft />
    </button>
    <div>
      {{ formatTitle(date) }}
    </div>
    <button
      class="button"
      aria-label="Следующий месяц"
      @click="$emit('onNext')"
    >
      <ChevronRight />
    </button>
  </div>
</template>

<style scoped>
.switcher {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.button {
  display: grid;
  place-items: center;
  border: none;
  border-radius: 8px;
  background-color: transparent;
  padding: 0;
  width: 32px;
  height: 32px;

  &:hover {
    background-color: oklch(0.94 0.015 250);
  }
}
</style>
