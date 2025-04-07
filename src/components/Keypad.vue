<template>
    <div class="keypad">
        <button
            v-for="key in keys"
            :key="key"
            @click="key && handleClick(key)"
            >
            {{ key }}
        </button>
    </div>
</template>

<script setup lang="ts">
    import { defineEmits } from 'vue'

    const emit = defineEmits<{
    (e: 'press', value: string): void
    }>()

    const keys = [
        'Clear', '^', '', '',
        '7', '8', '9', '/',
        '4', '5', '6', '*',
        '1', '2', '3', '-',
        '0', '00', '.', '+',
        '='
    ]
    
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
    font-size: 1.2rem;
    border: none;
    border-radius: 8px;
    background-color: #4e443c;
    cursor: pointer;
    transition: background-color 0.2s;
    }
    button:hover {
    background-color: #75604f;
    }
</style>