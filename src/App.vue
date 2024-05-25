<script setup>
import Preview from './components/Preview.vue'
</script>

<template>
  <Preview v-for="n in 10" :key="n" album-id="1" :photo-id="n-1"/>
</template>