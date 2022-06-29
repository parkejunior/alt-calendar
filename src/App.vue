<template>
  <header class="header">
    <h1>ALT-Calendar</h1>
  </header>

  <main style="display:flex;column-gap:10px;">
    <div>
      <CalendarWidget :startAt.sync="startDate" :businessDays.sync="businessDays" :holidays.sync="holidays">
      </CalendarWidget>
      <SettingsWidget @apply="applySettings"></SettingsWidget>
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
const businessDays = ref(false)
const holidays = ref(false)

const applySettings = (newSettings) => {
  startDate.value = newSettings.startDate
  businessDays.value = newSettings.businessDays
  holidays.value = newSettings.holidays
}
</script>

<style>
@import './assets/base.scss';
</style>
