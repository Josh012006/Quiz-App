<template>
    <progress :value="progress" max="100"></progress>
    <span style="margin: 10px;">{{ progress }} %</span>
</template>

<script setup>
import { defineProps } from 'vue'

defineProps({
    progress: Number
})
</script>

<style>

</style>