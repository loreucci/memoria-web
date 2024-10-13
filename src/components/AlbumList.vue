<script setup>
import { ref, onMounted } from 'vue';

defineEmits(['albumSelected'])

const albumList = ref(null)

onMounted(async () => {
    const res = await fetch(import.meta.env.VITE_MEMORIA_SERVER + "/albums")
    albumList.value = await res.json()
})

</script>

<template>
    <template v-if="albumList">
        <h2>Select an album</h2>
        <ul>
            <li v-for="album in albumList" :key="album.id" @click="$emit('albumSelected', album.id)">
                {{ album.name }}
            </li>
        </ul>
    </template>
    <template v-else>
        <h2>Loading albums...</h2>
    </template>
</template>

<style scoped>
ul {
    list-style: none;
}
li {
    border: 1px solid black;
    padding: 10px;
    margin: 10px;
}
</style>