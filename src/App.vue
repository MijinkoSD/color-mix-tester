<script setup lang="ts">
import { ref } from 'vue'
import ColorMix from './components/ColorMix.vue'
const colorMixCount = ref(1)
</script>

<template>
  <header>
    <h1>color-mix() tester</h1>
    <div class="add button">
      <div @click="colorMixCount++">+</div>
    </div>
  </header>

  <main>
    <ColorMix v-for="i in colorMixCount" :key="i" />
  </main>
</template>

<style scoped>
#app {
  width: fit-content;
}

header {
  position: fixed;
  display: flex;
  flex-direction: row;
  width: 45em;
  justify-content: space-between;
}

main {
  padding-top: 6em;
}

.button {
  margin: 1rem;
  display: block;
  padding: 0.5em;
  border-radius: 10px;
  cursor: pointer;
  color: white;
  background-color: hsl(103 59% 58% / 1);
  user-select: none;

  &:hover {
    background: hsl(103 59% 38% / 1);
  }

  &.add {
    border-radius: 50%;
    width: 1em;
    height: 1em;
    line-height: 1em;
    font-size: 2em;
    text-align: center;
  }
}
</style>
