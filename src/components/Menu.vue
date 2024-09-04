<script setup>
const props = defineProps({
  sizes: Array,
  activeSize: String
})
</script>

<template>
  <div>
    <ul>
      <li v-for="size in props.sizes" :key="size">
        <button @click="$emit('changeSize', size)" :class="{ active: size === props.activeSize }">
          <span>{{ size }}</span>
        </button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
div {
  display: flex;
  flex-direction: column;
  align-items: center;

  padding: 20px;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  box-shadow: 0 0 10px rgba(0, 0, 0);
  height: 100vh;
  width: 120px;
}
ul {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  height: 100%;
  justify-content: space-evenly;
  //margin: auto;
  //gap: 34px;
}
button {
  cursor: pointer;
  border: #1c4d47 6px solid;
  //border-radius: 50%;
  padding: 16px;
  //aspect-ratio: 1;
}
button span {
  font-size: 18px;
  font-weight: 700;
  letter-spacing: 1px;
  color: #1c4d47;
}
.active {
  background: green;
}
</style>
