<script setup>
import { ref, watch } from 'vue';
import Preview from './Preview.vue'
import Photo from './Photo.vue'
import Keyboard from './Keyboard.vue';

defineEmits(['back'])

const props = defineProps({
    albumId: {
        type: String,
        required: true
    }
})

const albumData = ref(null)
const currentPhoto = ref(null)

function reset() {
    albumData.value = null
    currentPhoto.value = null
}

watch(
    props,
    async (_newValue, _oldValue) => {
    reset()
        const res = await fetch(import.meta.env.VITE_MEMORIA_SERVER + "/albums/" + props.albumId)
        const res_data = await res.json()
        albumData.value = {
            albumName: res_data.name,
            albumId: props.albumId,
            albumSize: res_data.num_photos,
        }
    },
    {immediate: true}
)

function previewClicked(photoId) {
    currentPhoto.value = photoId
}

function navigation(e) {
    if (e.key == "ArrowRight" && currentPhoto.value < albumData.value.albumSize-1) {
        currentPhoto.value++;
    } else if (e.key == "ArrowLeft" && currentPhoto.value > 0) {
        currentPhoto.value--;
    }
}

</script>

<template>
    <button @click="$emit('back')">Change album</button><br>
    <template v-if="albumData">
        <Keyboard @keyup="navigation"></Keyboard>
        <p>Album: {{ albumData.albumName }}</p>
        <template v-if="currentPhoto != null">
            <Photo :album-id="albumData.albumId" :photo-id="currentPhoto"/>
        </template>
        <template v-else>
            <p>Click on a photo</p>
        </template>
        <div class="gallery">
            <Preview v-for="n in albumData.albumSize" :key="n" :album-id="albumData.albumId" :photo-id="n-1" @preview-clicked="previewClicked"/>
        </div>
    </template>
    <template v-else>
        <p>Loading...</p>
    </template>
</template>

<style scoped>
div.gallery {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    width: 100%;
}
</style>