<script setup>
import { ref } from 'vue'
import Card from './components/Card.vue'
import Score from './components/Score.vue'

const score = ref(0)

const handleCardFlip = (isFlipped) => {
  console.log('Card flipped:', isFlipped)
}

const handleCardStatusChange = (status) => {
  console.log('Card status changed:', status)
}
</script>

<template>
  <main class="main">
    <div class="header">
      ЗАПОМНИ СЛОВО
      <Score :score="score" />
    </div>
    <div class="content">
      <Card 
        number="02"
        word="armour-piercer"
        translation="бронебойный"
        @flip="handleCardFlip"
        @statusChange="handleCardStatusChange"
      />
    </div>
  </main>
</template>

<style scoped>
.main {
  background: #F0F4F8;
  width: 1440px;
  height: 100vh;
  display: flex;
  flex-direction: column;
}
.header {
  width: 1440px;
  height: 121px;  
  font-family: var(--font-family);
  font-weight: 700;
  font-size: 16px;
  line-height: 150%;
  letter-spacing: 0.12em;
  color: #222;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 40px;
}
.content {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
