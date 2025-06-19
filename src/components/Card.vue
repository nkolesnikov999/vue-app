<script setup>
import { ref } from 'vue'

const props = defineProps({
  number: {
    type: String,
    default: '01'
  },
  word: {
    type: String,
    default: 'unadmitted'
  },
  translation: {
    type: String,
    default: 'неподтвержденный'
  },
  isFlipped: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['flip', 'statusChange'])

const word = ref(props.word)
const translation = ref(props.translation)
const state = ref(props.isFlipped ? 'opened' : 'closed')
const status = ref('pending')

const handleCardClick = () => {
  if (state.value === 'closed') {
    state.value = 'opened'
    emit('flip', state.value)
  }
}

const handleStatusChange = (result) => {
  if (result === 'correct') {
    status.value = 'success'
  } else if (result === 'wrong') {
    status.value = 'fail'
  }
  emit('statusChange', result)
}
</script>

<template>
  <div class="card" :class="{ 'card--flipped': state === 'opened' }">
    <div class="card-number">{{ number }}</div>
    
    <div class="card-content">
      <div class="card-face card-face--front" v-show="state === 'closed'">
        <div class="word">{{ word }}</div>
      </div>
      <div class="card-face card-face--back" v-show="state === 'opened'">
        <div class="translation">{{ translation }}</div>
      </div>
    </div>
    
    <div class="card-bottom" v-show="state === 'closed'">
      <button class="flip-btn" @click="handleCardClick">ПЕРЕВЕРНУТЬ</button>
    </div>
    
    <div class="card-result-buttons" v-show="state === 'opened'">
      <button class="result-btn result-btn--wrong" @click.stop="handleStatusChange('wrong')">
        ✕
      </button>
      <button class="result-btn result-btn--correct" @click.stop="handleStatusChange('correct')">
        ✓
      </button>
    </div>
  </div>
</template>

<style scoped>
.card {
  background: #FFFFFF;
  border-radius: 20px;
  padding: 32px;
  width: 295px;
  height: 420px;
  display: flex;
  flex-direction: column;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
  position: relative;
  user-select: none;
}

.card-number {
  font-family: var(--font-family);
  font-weight: 400;
  font-size: 14px;
  line-height: 150%;
  color: #8B8B8B;
  margin-bottom: 24px;
}

.card-content {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.card-face {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

.word {
  font-family: var(--font-family);
  font-weight: 400;
  font-size: 20px;
  line-height: 150%;
  text-align: center;
  color: #2D2D2D;
}

.card-bottom {
  display: flex;
  justify-content: center;
  margin-top: 32px;
}

.flip-btn {
  background: #FFFFFF;
  border: 1px solid #E0E0E0;
  border-radius: 100px;
  padding: 12px 24px;
  font-family: var(--font-family);
  font-weight: 400;
  font-size: 14px;
  line-height: 150%;
  text-align: center;
  color: #8B8B8B;
  cursor: pointer;
  transition: all 0.2s ease;
}

.flip-btn:hover {
  border-color: #008BFE;
  color: #008BFE;
}

.translation {
  font-family: var(--font-family);
  font-weight: 400;
  font-size: 20px;
  line-height: 150%;
  text-align: center;
  color: #2D2D2D;
}

.card-result-buttons {
  display: flex;
  gap: 32px;
  justify-content: center;
  margin-top: 32px;
}

.result-btn {
  width: 52px;
  height: 52px;
  border: none;
  border-radius: 50%;
  font-size: 20px;
  font-weight: bold;
  color: white;
  cursor: pointer;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.result-btn:hover {
  transform: scale(1.1);
}

.result-btn--wrong {
  background: #F44336;
}

.result-btn--wrong:hover {
  background: #d32f2f;
}

.result-btn--correct {
  background: #4CAF50;
}

.result-btn--correct:hover {
  background: #388e3c;
}
</style> 