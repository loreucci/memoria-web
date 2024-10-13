<script setup>
import { ref } from 'vue'
import AlbumList from './components/AlbumList.vue';
import Gallery from './components/Gallery.vue'

const albumId = ref(null)
</script>

<template>
    <template v-if="albumId">
        <Gallery :album-id="albumId" @back="albumId = null"/>
    </template>
    <template v-else>
        <AlbumList @album-selected="(selectedAlbumId) => albumId = selectedAlbumId"></AlbumList>
    </template>
</template>