<script setup>
const color = 'red';
const id = "blue";
</script>

<template>
  <header :class="`color-${id}`">
    <h1>My App</h1>
  </header>
  <main :class="color">
    <h2>Welcome to My App</h2>
  </main>
  <footer>
    <p>Copyright 2025</p>
  </footer>
</template>

<style scoped>
</style>
