<script setup>
import { ref } from 'vue'
import Button from './Button.vue'

const props = defineProps({
  number: {
    type: String,
    default: '01'
  },
  word: {
    type: String,
    default: 'unadmitted'
  },
  isFlipped: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['flip', 'statusChange'])

const isCardFlipped = ref(props.isFlipped)

const handleFlip = () => {
  isCardFlipped.value = !isCardFlipped.value
  emit('flip', isCardFlipped.value)
}

const handleStatusChange = (status) => {
  emit('statusChange', status)
}
</script>

<template>
  <div class="card">
    <div class="card-number">{{ number }}</div>
    <div class="card-content">
      <div class="word">{{ word }}</div>
    </div>
    <div class="card-actions">
      <Button size="medium" @click="handleFlip">ПЕРЕВЕРНУТЬ</Button>
    </div>
  </div>
</template>

<style scoped>
.card {
  background: #FFFFFF;
  border-radius: 24px;
  padding: 40px;
  width: 375px;
  height: 536px;
  display: flex;
  flex-direction: column;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  position: relative;
}

.card-number {
  font-family: var(--font-family);
  font-weight: 400;
  font-size: 16px;
  line-height: 150%;
  color: #8B949E;
  margin-bottom: 20px;
}

.card-content {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.word {
  font-family: var(--font-family);
  font-weight: 400;
  font-size: 24px;
  line-height: 150%;
  text-align: center;
  color: #24292F;
}

.card-actions {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style> 