<template>
    <h4>Your balance</h4>
    <div id="balance">R${{ total }}</div>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    total: {
        type: Number,
        required: true
    }
})
</script>