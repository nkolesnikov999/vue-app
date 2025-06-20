<script setup>
import { ref } from 'vue'
import Card from './components/Card.vue'
import Score from './components/Score.vue'

const score = ref(0)

const cards = ref([
  {
    id: 1,
    number: "01",
    word: "armour-piercer",
    translation: "бронебойный",
    isFlipped: false,
    status: 'pending'
  },
  {
    id: 2,
    number: "02", 
    word: "dust-coat",
    translation: "пыльник",
    isFlipped: false,
    status: 'pending'
  },
  {
    id: 3,
    number: "03",
    word: "unadmitted",
    translation: "неподтвержденный",
    isFlipped: false,
    status: 'pending'
  },
  {
    id: 4,
    number: "04",
    word: "караван верблюдов",
    translation: "camel caravan",
    isFlipped: false,
    status: 'pending'
  }
])

const handleCardFlip = (cardId, isFlipped) => {
  const card = cards.value.find(c => c.id === cardId)
  if (card) {
    card.isFlipped = isFlipped === 'opened'
  }
  console.log('Card flipped:', cardId, isFlipped)
}

const handleCardStatusChange = (cardId, status) => {
  const card = cards.value.find(c => c.id === cardId)
  if (card) {
    card.status = status === 'correct' ? 'success' : 'fail'
    if (status === 'correct') {
      score.value += 1
    }
  }
  console.log('Card status changed:', cardId, status)
}
</script>

<template>
  <main class="main">
    <div class="header">
      ЗАПОМНИ СЛОВО
      <Score :score="score" />
    </div>
    <div class="content">
      <div class="cards-grid">
        <Card 
          v-for="card in cards"
          :key="card.id"
          :number="card.number"
          :word="card.word"
          :translation="card.translation"
          :isFlipped="card.isFlipped"
          @flip="(state) => handleCardFlip(card.id, state)"
          @statusChange="(status) => handleCardStatusChange(card.id, status)"
        />
      </div>
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
  padding: 40px;
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 32px;
  max-width: 1200px;
  width: 100%;
}
</style>
