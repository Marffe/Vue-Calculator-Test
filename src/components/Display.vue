<script setup lang="ts">
    defineProps<{ value: string }>()
</script>

<template>
    <div class="display">
        {{ value }}
    </div>
    </template>

<style scoped>
    .display {
        width: 75%;
        background-color: #222;
        color: white;
        font-size: 2rem;
        padding: 1rem;
        text-align: left;
        border-radius: 8px;
    }
</style>