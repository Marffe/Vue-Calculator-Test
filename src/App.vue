<template>
  <div id="app">
    <div class="calculator-box">
      <Display :value="display" />
      <Keypad @press="handlePress" />
    </div>
  </div>
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import { evaluate } from 'mathjs'
  import Display from './components/Display.vue'
  import Keypad from './components/Keypad.vue'

  const display = ref('')

function handlePress(key: string) {
  if (key === 'C') {
    display.value = ''
  } else if (key === '=') {
    try {
      display.value = evaluate(display.value).toString()
    } catch {
      display.value = 'Error'
    }
  } else {
    display.value += key
  }
}
</script>

<style scoped>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.calculator-box {
  background-color: #333;
  padding: 1rem;
  border-radius: 16px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: 300px;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
