<template>
  <form>
    <div class="card settings">
      <div>
        <label for="startDate" class="form-label">Start Date</label>
        <input type="date" id="startDate" v-model="startDate">
      </div>

      <div class="form-group">
        <label class="form-label">Workdays</label>
        <input type="checkbox" id="switchWorkdays" v-model="workdays" />
        <label for="switchWorkdays" class="switch-label">Workdays Days Toggle</label>
      </div>

      <div class="form-group" v-if="false">
        <label class="form-label">Holidays</label>
        <input type="checkbox" id="switchHolidays" v-model="holidays" />
        <label for="switchHolidays" class="switch-label">Holidays Toggle</label>
      </div>
    </div>
  </form>
</template>

<script setup>
import { ref, watchEffect } from "vue"

const emit = defineEmits(['apply'])
const startDate = ref('')
const workdays = ref(false)
const holidays = ref(false)

watchEffect(() => {
  emit('apply', {
    startDate: startDate.value,
    workdays: workdays.value,
    holidays: holidays.value,
  })
})
</script>

<style lang="scss">
.form-label {
  margin-bottom: 5px;
}

.form-group {
  input[type=checkbox] {
    height: 0;
    width: 0;
    visibility: hidden;
  }

  .switch-label {
    cursor: pointer;
    text-indent: -9999px;
    width: 70px;
    height: 30px;
    background: #00000033;
    display: block;
    border-radius: 100px;
    position: relative;
  }

  .switch-label:after {
    content: '';
    position: absolute;
    top: 5px;
    left: 5px;
    width: 20px;
    height: 20px;
    background: #fff;
    border-radius: 90px;
    transition: 0.3s;
  }

  input:checked+label {
    background: #000;
  }

  input:checked+label:after {
    left: calc(100% - 5px);
    transform: translateX(-100%);
  }

  .switch-label:active:after {
    width: 60px;
  }
}

.settings {
  margin-top: 10px;
  display: flex;
  justify-content: space-between;

  div * {
    display: block;
  }

  input {
    border-radius: 15px;
    border: 0;
    padding: 10px;
  }

  button {
    border-radius: 15px;
    border: 0;
    padding: 10px;
  }
}
</style>