<script setup>
import { ref, computed, watch } from 'vue'
import Banner from '@/components/Banner.vue'
import Card from '@/components/Card.vue'

const emojiArray = [
  128519, 128520, 128522, 128525, 128526, 128545, 128557, 128561, 128538, 128527, 128524, 129299,
  129297, 128563, 128565, 129313, 128123, 128169, 128571, 128584, 128150, 128149, 128148, 128420,
  128163, 127800, 127804, 127808, 127819, 127826, 11088, 127752, 128293, 128167, 10024
]
const props = defineProps({
  size: String
})
const numRandomEmojis = computed(() => {
  const parts = props.size.split('x') // розділяє з утворенням масиву
  const num1 = parseFloat(parts[0])
  const num2 = parseFloat(parts[1])

  return (num1 * num2) / 2
})

const emojis = computed(() => {
  const shuffledEmojis = [...emojiArray].toSorted(() => Math.random() - 0.5)
  const emojisPool = shuffledEmojis
    .slice(0, numRandomEmojis.value)
    .map((code) => String.fromCodePoint(code))

  return [...emojisPool, ...emojisPool].toSorted(() => Math.random() - 0.5)
})

const cardStates = ref([])

const timeInSeconds = ref(0) //Timer
const interval = ref(null)
const formattedTime = ref('')

function formatTime() {
  const minutes = Math.floor(timeInSeconds.value / 60)
  const seconds = timeInSeconds.value % 60
  return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
}
watch(emojis, (newEmojis) => {
  cardStates.value = new Array(newEmojis.length).fill(0)
  timeInSeconds.value = 0
  clearInterval(interval.value)
})

const prevCard = ref(null)
const handleCardClick = (index) => {
  cardStates.value[index] = 1

  if (timeInSeconds.value === 0) {
    interval.value = setInterval(() => {
      timeInSeconds.value++
      formattedTime.value = formatTime()
    }, 1000)
  }

  if (prevCard.value === null) {
    prevCard.value = index
  } else {
    if (emojis.value[prevCard.value] === emojis.value[index]) {
      prevCard.value = null
    } else {
      setTimeout(() => {
        cardStates.value[index] = 0
        cardStates.value[prevCard.value] = 0

        prevCard.value = null
      }, 500)
    }
    if (cardStates.value.every((state) => state === 1)) {
      clearInterval(interval.value)

      setTimeout(() => {
        alert('Congratulations! All cards are open!')
        startOver()
      }, 500)
    }
  }
}
const startOver = () => {
  for (let i = emojis.value.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[emojis.value[i], emojis.value[j]] = [emojis.value[j], emojis.value[i]]
  }
  cardStates.value = cardStates.value.map(() => 0) // Reset the card states to 0
  prevCard.value = null // Reset the previous card index
}
</script>

<template>
  <Banner v-if="!size" />
  <span> {{ formattedTime }} </span>
  <div class="board">
    <Card
      v-for="(row, index) in emojis"
      :key="index"
      :emoji="row"
      :cardState="cardStates[index]"
      @click="handleCardClick(index)"
    />
  </div>
</template>
<style scoped>
.board {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  margin: auto;
  gap: 10px;
}
.board:first-child {
  display: flex;
}
</style>
