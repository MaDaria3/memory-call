<script setup>
import { ref, computed } from 'vue'
defineProps({
  emoji: {
    type: String,
    required: true
  },
  cardState: Number
})
</script>

<template>
  <div class="card" :class="{ flipped: false && cardState === 1 }">
    <span v-if="cardState === 1">{{ emoji }}</span>
  </div>
</template>

<style scoped>
.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 160px;
  height: 160px;
  font-size: 64px;
  background-color: green;
  cursor: pointer;
  position: relative;
}

.card.flipped {
  position: relative;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}
.card.flipped {
  transform: rotateY(180deg);
}
.card.flipped {
  animation: flip 0.5s;
}
.card::after {
  content: '';
  position: absolute;
  transition: transform 0.5s;
  transform: rotateY(180deg);
  backface-visibility: hidden;
  background-color: lightgreen;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;

  opacity: 0;
  transition: opacity 0.8s;
}

.card.flipped::after {
  animation: flip 0.5s;
  opacity: 1;
}

@keyframes flip {
  0% {
    transform: rotateY(0deg);
  }
  100% {
    transform: rotateY(180deg);
  }
}
</style>
