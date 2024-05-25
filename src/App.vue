<script setup>
import { ref } from 'vue'
import Gallery from './components/Gallery.vue'

const albumId = ref("1")

function changeAlbum() {
    if (albumId.value == "1") {
        albumId.value = "0";
    } else {
      albumId.value = "1";
    }
}
</script>

<template>
    <button @click="changeAlbum">Change album (now: {{ albumId }})</button><br>
    <Gallery :album-id="albumId"/>
</template>