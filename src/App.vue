<template>
  <div id="app">
    <div class="calculator-box">
      <Display :value="display" />
      <Keypad @press="handlePress" />
    </div>
  </div>
</template>

<script setup lang="ts">
  // All Imported references
  import { ref } from 'vue'
  import { onMounted, onUnmounted } from 'vue'
  import { evaluate } from 'mathjs'
  import Display from './components/Display.vue'
  import Keypad from './components/Keypad.vue'

  // Operation variables
  const display = ref('')
  let lastOperator = ''
  let lastOperand = ''
  let justEvaluated = false // Checks if the last operation was valid

  function handlePress(key: string) {
    const operators = ['+', '-', '*', '/', '^'] // Operators, for anti breaking purposes
    const lastChar = display.value.slice(-1) // Backspace function

    if (key === 'C') { // Resets all variables
      display.value = ''
      lastOperator = ''
      lastOperand = ''
      justEvaluated = false

    } else if (key === '←') {
        display.value = display.value.slice(0, -1)

      // Stops weird behavior if you used ← and then try to use =
      if (justEvaluated) {
        lastOperator = ''
        lastOperand = ''
      }
      justEvaluated = false

    } else if (key === '=') {
      try { // Prevents uncomplete operations getting calculated
        if (!display.value || operators.includes(lastChar || '') || display.value === 'Error') return

        // 
        if (!justEvaluated && lastOperator && lastOperand) {
          return
        }

        // Checks if the last operation was valid and allow to keep reapeting it, only uses the last operator and number
        if (lastOperator && lastOperand) {
          const repeated = display.value + lastOperator + lastOperand
          display.value = evaluate(repeated).toString()
        } else {
          const match = display.value.match(/([\d.]+)([\+\-\*\/\^])([\d.]+)$/) // This looks so horrible and hard to read, but it works, Supports +, -, *, /, ^
          if (match) {
            const [, a, op, b] = match
            lastOperator = op
            lastOperand = b
            display.value = evaluate(display.value).toString()
            justEvaluated = true
          }
        }
      } catch {
        display.value = 'Error'
        justEvaluated = false
      }

    } else { // Normalices keyboard inputs so they behave exactly as the screen buttons
      if (operators.includes(key)) {
        if (display.value === '' && key !== '-') return
        if (operators.includes(lastChar || '')) {
          display.value = display.value.slice(0, -1) + key
        } else {
          display.value += key
        }
      } else {
        display.value += key
      }

      // Prevents = behavior if used a number or operator
      if (!operators.includes(key)) {
        if (justEvaluated) {
          lastOperator = ''
          lastOperand = ''
        }
        justEvaluated = false
      }
    }
  }

  function handleKeyPress(e: KeyboardEvent) {
    e.preventDefault() // Prevents default behavior if the page was reloded

    const key = e.key
    // Keyboard functions
    if ('0123456789'.includes(key)) {
      handlePress(key)
    } else if (['+', '-', '*', '/', '.', '^'].includes(key)) {
      handlePress(key)
    } else if (key === 'Enter') {
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
  background-color: #3a3644;
  padding: 1rem;
  border-radius: 16px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: 315px;
}
</style>
