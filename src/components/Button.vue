<script setup>
const props = defineProps({
  size: {
    type: String,
    default: 'large', // large, medium, small
    validator: (value) => ['large', 'medium', 'small'].includes(value)
  }
})

const getButtonClasses = () => {
  return `button button--${props.size}`
}
</script>

<template>
  <button :class="getButtonClasses()">
    <slot></slot>
  </button>
</template>

<style scoped>
.button {
  border: none;
  border-radius: 100px;
  background: #008bfe;
  font-family: var(--font-family);
  font-weight: 400;
  line-height: 150%;
  text-align: center;
  color: #fff;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  transition: all 0.2s ease;
}

.button:hover {
  background: #0066cc;
  transform: translateY(-1px);
}

.button--large {
  width: 335px;
  height: 68px;
  padding: 16px 10px;
  font-size: 24px;
}

.button--medium {
  width: 240px;
  height: 56px;
  padding: 14px 24px;
  font-size: 16px;
}

.button--small {
  width: 160px;
  height: 48px;
  padding: 12px 20px;
  font-size: 14px;
}
</style>