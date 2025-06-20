<script setup>
import { ref, watch } from 'vue'

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

const state = ref(props.isFlipped ? 'opened' : 'closed')
const status = ref('pending')

// Watch for external isFlipped changes
watch(() => props.isFlipped, (newValue) => {
  state.value = newValue ? 'opened' : 'closed'
})

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
  <div class="card" :class="{ 
    'card--flipped': state === 'opened',
    'card--success': status === 'success',
    'card--fail': status === 'fail'
  }">
    <div class="card-number">{{ number }}</div>
    
    <!-- Status indicator -->
    <div class="card-status" v-show="status !== 'pending'">
      <div class="status-btn status-btn--wrong" v-if="status === 'fail'">
        ✕
      </div>
      <div class="status-btn status-btn--correct" v-if="status === 'success'">
        ✓
      </div>
    </div>
    
    <div class="card-content">
      <div class="card-face card-face--front" v-show="state === 'closed'">
        <div class="word">{{ word }}</div>
      </div>
      <div class="card-face card-face--back" v-show="state === 'opened' && status === 'pending'">
        <div class="translation">{{ translation }}</div>
      </div>
      <div class="card-face card-face--completed" v-show="status !== 'pending'">
        <div class="word">{{ word }}</div>
      </div>
    </div>
    
    <div class="card-bottom" v-show="state === 'closed'">
      <button class="flip-btn" @click="handleCardClick">ПЕРЕВЕРНУТЬ</button>
    </div>
    
    <div class="card-result-buttons" v-show="state === 'opened' && status === 'pending'">
      <button class="result-btn result-btn--wrong" @click.stop="handleStatusChange('wrong')">
        ✕
      </button>
      <button class="result-btn result-btn--correct" @click.stop="handleStatusChange('correct')">
        ✓
      </button>
    </div>
    
    <div class="card-completed" v-show="status !== 'pending'">
      <div class="completed-text">ЗАВЕРШЕНО</div>
    </div>
  </div>
</template>

<style scoped>
.card {
  background: #F8F9FA;
  border-radius: 14px;
  padding: 20px;
  width: 250px;
  height: 376px;
  display: flex;
  flex-direction: column;
  box-shadow: 0 3px 16px rgba(0, 0, 0, 0.08);
  position: relative;
  user-select: none;
  border: 1px solid #E9ECEF;
}

.card-number {
  font-family: var(--font-family);
  font-weight: 500;
  font-size: 14px;
  line-height: 120%;
  color: #6C757D;
  margin-bottom: 18px;
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
  font-weight: 500;
  font-size: 20px;
  line-height: 130%;
  text-align: center;
  color: #212529;
}

.card-bottom {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.flip-btn {
  background: transparent;
  border: 1px solid #DEE2E6;
  border-radius: 7px;
  padding: 8px 16px;
  font-family: var(--font-family);
  font-weight: 500;
  font-size: 10px;
  line-height: 120%;
  text-align: center;
  color: #6C757D;
  cursor: pointer;
  transition: all 0.2s ease;
  letter-spacing: 0.4px;
}

.flip-btn:hover {
  border-color: #ADB5BD;
  color: #495057;
  background: #FFFFFF;
}

.translation {
  font-family: var(--font-family);
  font-weight: 500;
  font-size: 20px;
  line-height: 130%;
  text-align: center;
  color: #212529;
}

.card-face--back {
  border: 1px solid #DEE2E6;
  border-radius: 10px;
  padding: 20px;
  margin: 18px 0;
}

.card-result-buttons {
  display: flex;
  gap: 20px;
  justify-content: center;
  margin-top: 20px;
}

.result-btn {
  width: 42px;
  height: 42px;
  border: none;
  border-radius: 50%;
  font-size: 16px;
  font-weight: bold;
  color: white;
  cursor: pointer;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 7px rgba(0, 0, 0, 0.15);
}

.result-btn:hover {
  transform: scale(1.05);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.result-btn--wrong {
  background: #DC3545;
}

.result-btn--wrong:hover {
  background: #C82333;
}

.result-btn--correct {
  background: #28A745;
}

.result-btn--correct:hover {
  background: #218838;
}

/* Status indicator styles */
.card-status {
  position: absolute;
  top: 20px;
  right: 20px;
  z-index: 10;
}

.status-btn {
  width: 28px;
  height: 28px;
  border: none;
  border-radius: 50%;
  font-size: 12px;
  font-weight: bold;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 7px rgba(0, 0, 0, 0.15);
}

.status-btn--wrong {
  background: #DC3545;
}

.status-btn--correct {
  background: #28A745;
}

/* Completed state styles */
.card-face--completed {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

.card-completed {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.completed-text {
  font-family: var(--font-family);
  font-weight: 500;
  font-size: 10px;
  line-height: 120%;
  text-align: center;
  color: #6C757D;
  letter-spacing: 0.4px;
}

/* Card state classes */
.card--fail {
  /* Could add specific fail state styles if needed */
}

.card--success {
  /* Could add specific success state styles if needed */
}
</style> 