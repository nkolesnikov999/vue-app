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
  background: #008bfe;
  padding: 16px 10px;
  font-family: var(--font-family);
  font-size: 24px;
  font-weight: 400;
  line-height: 150%;
  text-align: center;
  color: #fff;
  cursor: pointer;
  height: 68px;
  display: flex;
  width: 335px;
  gap: 10px;
  align-items: center;
  justify-content: center;
}
</style>