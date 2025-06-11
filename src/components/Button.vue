<script setup></script>

<template>
  <button class="button">
    <slot></slot>
  </button>
</template>

<style scoped>
.button {
  border: none;
  border-radius: 100px;
  background: #008BFE;
  padding: 16px 10px 16px 16px;
  font-family: var(--font);
  font-size: 20px;
  font-weight: 600;
  color: var(--color-primary);
  cursor: pointer;
  height: 68px;
  display: flex;
  width: 335px;
  gap: 10px;
  align-items: center;
  justify-content: center;
}
</style>