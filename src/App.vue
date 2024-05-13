<script setup>
import Preview from './components/Preview.vue'
</script>

<template>
  <Preview v-for="n in 10" :key="n" :photo_id="n-1" :album_id="1" :preview_size="200"/>
</template>