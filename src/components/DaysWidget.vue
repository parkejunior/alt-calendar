<template>
  <section class="card list">
    <div class="list-item" v-for="i in 5">
      <div>
        <div class="day-title">{{ i * 3 + Math.floor(Math.random() * 3) }}</div>
        <div class="day-subtitle">Quarta-feita</div>
      </div>
      <div>

      </div>
    </div>
  </section>
</template>

<script setup>
</script>

<style>
.day-title {
  font-size: 38px;
  color: #555;
}

.day-subtitle {
  font-size: 12px;
  color: #999;
}
</style>