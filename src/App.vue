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
  import { onMounted, onUnmounted } from 'vue'
  import { evaluate } from 'mathjs'
  import Display from './components/Display.vue'
  import Keypad from './components/Keypad.vue'

  const display = ref('')
  let lastOperator = ''
  let lastOperand = ''

function handlePress(key: string) {
  // Clear to calculator
  if (key === 'C') {
    display.value = ''
    lastOperator = ''
    lastOperand = ''
  } else if (key === '←') {
    display.value = display.value.slice(0, -1)
  } 
    else if (key === '=') { // To repeat the last operation
    try {
      // If the last operation exists and the display ends in a number:
      if (lastOperator && lastOperand) {
        display.value = evaluate(display.value + lastOperator + lastOperand).toString()
      } else {
        // First time = is pressed → extract the last operation
        const match = display.value.match(/([\d.]+)([\+\-\*\/\^])([\d.]+)$/) // This looks so horrible and hard to read, but it works, Supports +, -, *, /, ^
        if (match) {
          const [, a, op, b] = match
          lastOperator = op
          lastOperand = b
          display.value = evaluate(display.value).toString()
        }
      }
    } catch {
      display.value = 'Error'
    }
  } else {
    display.value += key
    // Reset repeat-op memory if user types something new
    if (!['+', '-', '*', '/', '^'].includes(key)) {
      lastOperator = ''
      lastOperand = ''
    }
  }
}

// Keyboard support
function handleKeyPress(e: KeyboardEvent) {
  const key = e.key

  if ('0123456789'.includes(key)) {
    handlePress(key)
  } else if (['+', '-', '*', '/', '.', '^', '00'].includes(key)) {
    handlePress(key)
  } else if (key === 'Enter' || key === '=') {
    handlePress('=')
  } else if (key === 'Backspace') {
    handlePress('←')
  } else if (key === 'Escape') {
    handlePress('C')
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeyPress)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeyPress)
})
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
