<script setup>
import { ref, onMounted } from 'vue'
import Card from './components/Card.vue'
import Score from './components/Score.vue'

const score = ref(0)
const cards = ref([])

const loadWords = async () => {
  try {
    const response = await fetch('http://localhost:8080/api/random-words')
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    const data = await response.json()
    
    // Берем первые 10 элементов или все, если меньше 10
    const wordsToUse = data.slice(0, 10)
    
    cards.value = wordsToUse.map((item, index) => ({
      id: index + 1,
      number: String(index + 1).padStart(2, '0'),
      word: item.word,
      translation: item.translation,
      isFlipped: false,
      status: 'pending'
    }))
  } catch (error) {
    console.error('Error loading words:', error)
    // Fallback данные в случае ошибки
    cards.value = [
      {
        id: 1,
        number: "01",
        word: "armour-piercer",
        translation: "бронебойный",
        isFlipped: false,
        status: 'pending'
      }
    ]
  }
}

onMounted(() => {
  loadWords()
})

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
  min-height: 100vh;
  width: 100vw;
  min-width: 1400px;
  display: flex;
  flex-direction: column;
  overflow-x: auto;
}
.header {
  width: 100%;
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
  box-sizing: border-box;
}
.content {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 40px 100px;
  box-sizing: border-box;
  min-width: 1400px;
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 24px;
  width: 100%;
}

@media (max-width: 1500px) {
  .main {
    min-width: 1200px;
  }
  .content {
    min-width: 1200px;
  }
  .cards-grid {
    gap: 20px;
  }
}

@media (max-width: 1300px) {
  .main {
    min-width: 1000px;
  }
  .content {
    min-width: 1000px;
  }
  .cards-grid {
    gap: 18px;
  }
}

@media (max-width: 1024px) {
  .cards-grid {
    grid-template-columns: repeat(4, 1fr);
    gap: 14px;
  }
  .content {
    padding: 40px 100px;
  }
}

@media (max-width: 850px) {
  .cards-grid {
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
  }
  .content {
    padding: 30px 80px;
  }
}

@media (max-width: 768px) {
  .cards-grid {
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
  }
  .content {
    padding: 20px 60px;
  }
  .header {
    padding: 0 60px;
  }
}

@media (max-width: 480px) {
  .cards-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
  }
  .content {
    padding: 20px 40px;
  }
  .header {
    padding: 0 40px;
  }
}
</style>
