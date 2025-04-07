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
        width: 90%;
        height: 2.2rem;
        background-color: #414d4a;
        color: rgb(238, 238, 227);
        font-size: 2rem;
        padding: 1rem;
        text-align: right;
        border-radius: 8px;
        align-self: center;
    }
</style>