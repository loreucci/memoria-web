<script setup>
import { onMounted, onUnmounted } from 'vue';

const emit = defineEmits(['keyup'])

function keyUpHandler(e) {
    emit('keyup', e);
}

onMounted(() => {
    window.addEventListener('keyup', keyUpHandler);
})

onUnmounted(() => {
    window.removeEventListener('keyup', keyUpHandler);
})

</script>

<template>
    <div></div>
</template>

<style scoped>
div {
  display: none;
}
</style>