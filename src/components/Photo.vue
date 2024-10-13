<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
    albumId: {
        type: String,
        required: true
    },
    photoId: {
        type: Number,
        required: true
    }
})

const photoSize = 500

function getDivSize() {
    return (photoSize + 6) + "px"
}

function getPhotoSize() {
    return photoSize + "px"
}

const imgurl = ref(null)

watch(
    props,
    (_newValue, _oldValue) => {
        imgurl.value = null
        imgurl.value = import.meta.env.VITE_MEMORIA_SERVER + "/albums/" + props.albumId + "/photos/" + props.photoId
    },
    {immediate: true}
)

// fullscreen
const photoimg = ref(null)

function go_full(event) {
    let elem = photoimg.value;
    elem.requestFullscreen = elem.requestFullscreen || elem.mozRequestFullscreen
          || elem.msRequestFullscreen || elem.webkitRequestFullscreen;
    elem.requestFullscreen().then({}).catch(err => {
      alert(`Error attempting to enable full-screen mode: ${err.message} (${err.name})`);
    });
}

</script>

<template>
    <div class="photo" @click="go_full">
        <template v-if="imgurl">
            <img :src="imgurl" ref="photoimg"/>
        </template>
        <template v-else>
            <p>Loading...</p>
        </template>
    </div>
</template>

<style scoped>
div.photo {
    margin: 5px;
    padding: 2px;
    width: v-bind('getDivSize()');
    height: v-bind('getDivSize()');
    border: solid 1px;
    align-content: center;
}
img {
    margin-left: auto;
    margin-right: auto;
    display: block;
    max-width: v-bind('getPhotoSize()');
    max-height: v-bind('getPhotoSize()');
}
</style>