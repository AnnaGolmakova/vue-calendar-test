<script setup lang="ts">
import Calendar from "./components/Calendar.vue";
import { ref } from "vue";

const showDatesOutOfMonth = ref(false);
const showWeekdays = ref(true);
const highlightwWeekends = ref(true);

const locale = ref("ru-RU");
</script>

<template>
  <main class="layout">
    <h1>Календарь</h1>

    <Calendar
      :locale="locale"
      :highlightw-weekends="highlightwWeekends"
      :show-weekdays="showWeekdays"
      :show-dates-out-of-month="showDatesOutOfMonth"
      @change="console.log"
    />

    <div class="controls">
      <label for="weekdays">
        <input type="checkbox" id="weekdays" v-model="showWeekdays" />
        Показывать дни недели
      </label>
      <label for="weekends">
        <input type="checkbox" id="weekends" v-model="highlightwWeekends" />
        Подсвечивать выходные дни
      </label>
      <label for="outdays">
        <input type="checkbox" id="outdays" v-model="showDatesOutOfMonth" />
        Показывать даты других месяцев
      </label>

      <label for="locales-select">
        Локализация:
        <select name="pets" id="locales-select" v-model="locale">
          <option value="ru-RU">Russian</option>
          <option value="en-GB">English (GB)</option>
          <option value="en-US">English (US)</option>
          <option value="es-ES">Spanish</option>
          <option value="pt-PT">Portugal</option>
          <option value="de-DE">German (Germany)</option>
          <option value="ja-JA">Japan</option>
        </select>
      </label>
    </div>
  </main>
</template>

<style scoped>
.layout {
  display: flex;
  flex-direction: column;
  align-items: left;
  min-height: 400px;
}

.controls {
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-top: 32px;
  font-size: 14px;
}
</style>
