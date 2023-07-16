<template>
  <div class="memory-game">
    <h1 class="title">Free Cat Memory Game</h1>
    <div class="card-grid">
      <Card
        v-for="(cardId, index) in shuffledCards"
        :key="`${cardId}-${index}`"
        :cardId="cardId"
        :index="index"
        :isFlipped="isCardFlipped(index)"
        :isMatched="isCardMatched(cardId)"
        @clickCard="selectCard(cardId, index)"
      />
    </div>
    <div class="counter-container">
      <div class="counter">Tries: {{ tries }}</div>
      <div class="counter">Matches: {{ matches.length }}</div>
      <div class="counter">Score: {{ score }}/100</div>
      <button class="reset-button" @click="resetGame">Reset</button>
    </div>
    <div class="ad" @click="showAd">
      <p>Click <a href="#" @click.prevent="showAlert">here</a> to win a free cat! 😺</p>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'
import Card from './components/Card.vue'

const cardIds = ref([1, 1, 2, 2, 3, 3, 4, 4, 5, 5, 6, 6])
let shuffledCards = ref([...cardIds.value].sort(() => Math.random() - 0.5))
const selectedCards = reactive([])
const matches = reactive([])
const tries = ref(0)

const isCardFlipped = (index) => {
  return (
    selectedCards.some((card) => card.index === index) ||
    matches.includes(shuffledCards.value[index])
  )
}

const isCardMatched = (cardId) => {
  return matches.includes(cardId)
}

const selectCard = (cardId, index) => {
  if (isCardSelected(index)) {
    return
  }

  if (selectedCards.length < 2) {
    selectedCards.push({ cardId, index })
  }

  if (selectedCards.length === 2) {
    tries.value++
    checkMatch()
  }
}

const isCardSelected = (index) => {
  return selectedCards.some((card) => card.index === index)
}

const checkMatch = () => {
  const [firstCard, secondCard] = selectedCards

  if (firstCard.cardId === secondCard.cardId) {
    matches.push(firstCard.cardId)
    clearSelectedCards()
  } else {
    setTimeout(clearSelectedCards, 1000)
  }
}

const clearSelectedCards = () => {
  selectedCards.splice(0, selectedCards.length)
}

const resetGame = () => {
  shuffledCards.value = [...cardIds.value].sort(() => Math.random() - 0.5)
  selectedCards.splice(0, selectedCards.length)
  matches.splice(0, matches.length)
  tries.value = 0
}

const score = computed(() => {
  if (matches.length === 0 || tries.value === 0) {
    return 0
  }
  const accuracy = (matches.length / tries.value) * 100
  return Math.round(accuracy)
})

const showAlert = () => {
  alert(
    'You just clicked on a suspicious link!!!\nPlease complete the Security Awareness Essentials Course.'
  )
}
</script>

<style scoped>
.memory-game {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  height: 100vh;
  background: url('/public/images/background.jpg') no-repeat center center fixed;
  background-size: cover;
  margin: 0;
  padding: 0;
  outline: none;
  border: none;
  overflow-y: hidden;
}

.card-grid {
  max-width: 1300px;
  margin: 0 auto;
  display: grid;
  gap: 20px;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}

.counter-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
  margin-top: 20px;
}

.counter {
  font-size: 24px;
  background-color: #1e90ff;
  padding: 10px;
  border-radius: 5px;
  text-shadow: 1px 1px 2px rgba(255, 255, 255, 0.5);
  color: white;
}

.reset-button {
  padding: 10px 20px;
  font-size: 24px;
  border-radius: 5px;
  background-color: #1e90ff;
  color: white;
  font-size: 24px;
  cursor: pointer;
}

.ad {
  margin-top: 20px;
  padding: 20px;
  border: 5px dashed #1e90ff;
  border-radius: 10px;
  text-align: center;
  font-size: 24px;
  animation: flash 2s linear infinite;
  cursor: pointer;
}

@keyframes flash {
  0% {
    background-color: transparent;
  }
  50% {
    background-color: #1e90ff;
  }
  100% {
    background-color: transparent;
  }
}

.title {
  font-size: 60px;
  text-align: center;
  color: #ff6347;
  text-shadow: 2px 2px #1e90ff;
  margin-bottom: 20px;
}
</style>
