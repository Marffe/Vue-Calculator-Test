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
        height: 2.4rem;
        background-color: #C3DADA;
        color: #131C1C;
        font-size: 3rem;
        font-family: 'DigitalDisplay', monospace;
        padding: 0.3rem 0.8rem 0.3rem 0.8rem;
        line-height: 2.2rem;
        text-align: right;
        border-radius: 6px;
        align-self: center;
        overflow: hidden;
        white-space: nowrap;
        word-break: keep-all;
        cursor: default;
    }
</style>