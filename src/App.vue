<script setup>
import Menu from './components/Menu.vue'
import Board from './components/Board.vue'
import { ref } from 'vue'

const boardSizes = ['2x4', '3x4', '4x4']
const activeSize = ref('')
function changeBoardState(size) {
  activeSize.value = size
}
</script>

<template>
  <Menu :sizes="boardSizes" :active-size="activeSize" @change-size="changeBoardState" />
  <Board :size="activeSize" />
</template>

<style scoped></style>
