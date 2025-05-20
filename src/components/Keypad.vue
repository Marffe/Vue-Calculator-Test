<template>
    <div class="keypad">
        <button
            v-for="key in keys"
            :key="key"
            @click="key && handleClick(key)"
            :class="{
                'clear-btn': key === 'Clear',
                'equal-btn': key === '=',
                'operator-btn': ['/', '*', '-', '+', '^'].includes(key),
                'number-btn': !isNaN(Number(key)) || key === '00',
                'special-btn': key === '←' || key === '.',
                'magic-btn': key === ''
            }">
            <img v-if="key === ''" src="../assets/svg/magic.svg" alt="magic" class="magic-icon"/>
                {{ key }}
        </button>
    </div>
</template>

<script setup lang="ts">
    import { defineEmits } from 'vue'

    const emit = defineEmits<{
    (e: 'press', value: string): void
    }>()

    const keys = [ 'Clear', '', '^', '←', '7', '8', '9', '/', '4', '5', '6', '*', '1', '2', '3', '-', '0', '00', '.', '+', '=']
    // Uses mathjs library to handle functions
    function handleClick(key: string) {
    emit('press', key)
    if (key === 'Clear') {
    emit('press', 'C')  // sends 'C' instead of 'Clear'
    }
}
</script>
    
<style scoped>
    .keypad {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 0.5rem;
    margin-top: 1rem;
    }
    button {
    padding: 1rem;
    font-size: 1.4rem;
    font-family: 'ArchivoBold', Arial, sans-serif;
    border: none;
    border-radius: 8px;
    background-color: #28212F;
    cursor: pointer;
    transition: background-color;
    }
    img { width: 1.5rem; height: 1.5rem;}

    button:hover { background-color: #3A2D49; }
    .clear-btn { background-color: #bb587e; font-size: 1.2rem; }
    .clear-btn:hover { background-color: #682e49; }

    .equal-btn {
        grid-column: span 2;
        font-weight: bold;
    }

    .number-btn { background-color: #686868; color: #ffffff;}
    .number-btn:hover { background-color: #675c6b; }

    .magic-btn { background-color: #28212F; color: #ffffff;}
    .magic-btn:hover { background-color: #3A2D49; }
</style>