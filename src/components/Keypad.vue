<template>
    <div class="keypad">
        <button v-for="key in keys" :key="key" @click="handleClick(key)">
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
    '7', '8', '9', '/',
    '4', '5', '6', '*',
    '1', '2', '3', '-',
    '0', '.', '+', '=',
    'C'
    ]
    
    function handleClick(key: string) {
    emit('press', key)
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
    background-color: #eee;
    cursor: pointer;
    transition: background-color 0.2s;
    }
    button:hover {
    background-color: #ccc;
    }
    </style>