<script setup>
import { ref } from 'vue'

const now = ref(new Date().toLocaleDateString('ru-RU', {
  year: 'numeric',
  month: 'long',
  day: 'numeric',
}))
</script>

<template>

  <main>
    <p>Сегодня: {{ now }}</p>
  </main>

</template>

<style scoped>
</style>
