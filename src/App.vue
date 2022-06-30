<template>
  <header class="header">
    <h1>ALT-Calendar</h1>
  </header>

  <main class="app-content">
    <div>
      <CalendarWidget 
        :startAt.sync="startDate" 
        :workdays.sync="workdays" 
        :holidays.sync="holidays"
      ></CalendarWidget>
      <SettingsWidget @apply="applySettings"></SettingsWidget>
      <footer class="app-footer">
        by <a href="https://github.com/parkejunior" target="_blank">parkejunior</a>
      </footer>
    </div>
    <DaysWidget v-if="false"></DaysWidget>
  </main>
</template>

<script setup>
import CalendarWidget from '@/components/CalendarWidget.vue'
import DaysWidget from '@/components/DaysWidget.vue'
import SettingsWidget from '@/components/SettingsWidget.vue'
import { ref } from "vue"

const startDate = ref('')
const workdays = ref(false)
const holidays = ref(false)

const applySettings = (newSettings) => {
  startDate.value = newSettings.startDate
  workdays.value = newSettings.workdays
  holidays.value = newSettings.holidays
}
</script>

<style>
@import './assets/base.scss';

.app-content {
  display: flex;
  column-gap: 10px;
}

.app-footer, 
.app-footer * {
  color: #ffffff66;
  font-style: italic;
}

.app-footer:hover * {
  color: #fff;
}
</style>
