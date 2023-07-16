<template>
  <div class="card" :class="{ flipped: isFlipped || isMatched }" @click="handleClick">
    <div class="card-side card-front">
      <img :src="cardBackImageUrl" alt="Card Back" />
    </div>
    <div class="card-side card-back">
      <img :src="catImageUrl" alt="Card Image" />
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  cardId: Number,
  index: Number,
  isFlipped: Boolean,
  isMatched: Boolean
})

const cardBackImageUrl = '/images/cardBack.jpeg'
const catImageUrl = `/images/cat${props.cardId}.jpeg`

const emit = defineEmits(['clickCard'])

const handleClick = () => {
  if (props.isMatched || props.isFlipped) {
    return
  }

  emit('clickCard', props.cardId, props.index)
}
</script>

<style scoped>
.card {
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
  border-radius: 4px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  perspective: 1000px;
  transform-style: preserve-3d;
  transition: transform 0.3s;
}

.card.flipped {
  transform: rotateY(180deg);
}

.card-side {
  width: 100%;
  height: 100%;
  position: absolute;
  backface-visibility: hidden;
  border-radius: 4px;
}

.card-front {
  background: #1e90ff;
  transform: rotateY(0deg);
}

.card-back {
  background: #ff6347;
  transform: rotateY(180deg);
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
