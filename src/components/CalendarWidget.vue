<template>
  <div class="card calendar-title">
    <button class="calendar-switcher calendar-prev" @click="previousMonth">&#8249;</button>
    <h2>{{ currentMonthName }}</h2>
    <button class="calendar-switcher calendar-next" @click="nextMonth">&#8250;</button>
  </div>

  <table class="calendar card">
    <thead>
      <tr>
        <th>Sun</th>
        <th>Mon</th>
        <th>Tue</th>
        <th>Wed</th>
        <th>Thu</th>
        <th>Fri</th>
        <th>Sat</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="week in month">
        <td v-for="date in week" :class="dayClasses(date)">
          {{ date.day }}
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { ref, onMounted, watch, watchEffect } from "vue"
import c from 'calendar'

const props = defineProps({
  startAt: {
    type: String,
  },
  workdays: Boolean,
  holidays: Boolean,
})

const currentMonthName = ref('')
const currentMonth = ref(0)
const month = ref([])

const months = [
  'January',
  'February',
  'March',
  'April',
  'May',
  'June',
  'July',
  'August',
  'September',
  'October',
  'November',
  'December'
]

currentMonth.value = 6
const currentYear = 2022
const calendar = new c.Calendar();
const today = new Date;
let startDate = new Date
let workdays = false;
let holidays = false;

watchEffect(() => {
  startDate = new Date(props.startAt || null);
  workdays = props.workdays || false;
  holidays = props.holidays || false;

  updateCalendar()
})

onMounted(updateCalendar)

function updateCalendar() {
  currentMonthName.value = months[currentMonth.value - 1]

  const monthDates = calendar.monthDates(currentYear, currentMonth.value - 1)

  month.value = monthDates.map(week => week.map(day => {
    const dateObj = new Date(day);

    return {
      day: dateObj.getDate(),
      month: dateObj.getMonth() + 1,
      year: dateObj.getFullYear(),
    }
  }))
}

const previousMonth = () => {
  if (currentMonth.value > 1) currentMonth.value--
}

const nextMonth = () => {
  if (currentMonth.value < 12) currentMonth.value++
}

const dayClasses = (date) => {
  return [
    date.month == currentMonth.value || 'text-muted',
    !isToday(date.year, date.month, date.day) || 'day-current',
    !isActive(date.year, date.month, date.day) || 'day-active',
  ]
}

const isToday = (year, month, day) => {
  const date = new Date(year, month - 1, day)
  today.setHours(0, 0, 0, 0)

  return today.getTime() === date.getTime()
}

const isActive = (year, month, day) => {
  const currentDate = new Date(year, month - 1, day)
  let active = false

  if (
    currentDate.getTime() > startDate.getTime() && // Active on after startDate
    (!workdays || !isWorkday(currentDate)) &&      // Active if is workday
    datesBetween(currentDate, startDate) % 2 === 0 // Active each 2 days
  ) {
    active = true
  }
  
  return active
}

function datesBetween(date1, date2) {
  date1 = new Date(date1)
  date2 = new Date(date2)

  return Math.floor((date1 - date2) / (1000 * 3600 * 24))
}

function isWorkday(date) {
  return date.getDay() == 6 || date.getDay() == 0
}
</script>

<style>
.text-muted {
  color: #ffffff66;
}

.calendar .day-current {
  background-color: #ffffff !important;
  color: black;
}

.calendar .day-active {
  background-color: #00000033;
}

.calendar td {
  padding: 10px 15px;
  text-align: center;
  border-radius: 20px;
}

.calendar td:hover {
  background-color: #00000066;
  cursor: pointer;
}

.calendar-title {
  margin-bottom: 10px;
  padding: 0!important;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.calendar-title h2 {
  margin: 0;
}

button.calendar-switcher {
  border: 0;
}

.calendar-switcher {
  background-color: #00000066;
  color: white;
  padding: 10px 20px;
  font-size: 28px;
  cursor: pointer;
}

.calendar-switcher:hover {
  background-color: #00000099;
}

.calendar-switcher:active {
  background-color: #000;
}

.calendar-prev {
  border-radius: 15px 0px 0px 15px;
}

.calendar-next {
  border-radius: 0px 15px 15px 0px;
}

@media only screen and (max-width: 500px) {
  .calendar td {
    padding: 7px 10px;
    font-size: .85rem;
  }
}
</style>