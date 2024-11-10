<script setup>
import { ref, watch } from 'vue';

defineEmits(['closePhoto', 'previousPhoto', 'nextPhoto'])

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

function goFullscreen(event) {
    let elem = photoimg.value;
    elem.requestFullscreen = elem.requestFullscreen || elem.mozRequestFullscreen
          || elem.msRequestFullscreen || elem.webkitRequestFullscreen;
    elem.requestFullscreen().then({}).catch(err => {
      alert(`Error attempting to enable full-screen mode: ${err.message} (${err.name})`);
    });
}

</script>

<template>
    <div class="photo">
        <div class="content">
            <span class="navigation close" @click="$emit('closePhoto')">&times;</span>
            <span class="navigation full" @click="goFullscreen()">[]</span>
            <template v-if="imgurl">
                <img :src="imgurl" ref="photoimg"/>
            </template>
            <template v-else>
                <p>Loading...</p>
            </template>
            <a class="navigation prev" @click="$emit('previousPhoto')">&#10094;</a>
            <a class="navigation next" @click="$emit('nextPhoto')">&#10095;</a>
        </div>
    </div>
</template>

<style scoped>
div.photo {
    position: absolute;
    z-index: 1;
    left: 0;
    top: 0;
    margin: 0px;
    padding: 0px;
    width: 100%;
    height: 100%;
    align-content: center;
    background-color: rgba(0,0,0,0.9);
    text-align: center;
}
.content {
    position: relative;
    background-color: #fefefe;
    margin: auto;
    padding: 0;
    max-width: v-bind('getPhotoSize()');
    max-height: v-bind('getPhotoSize()');
    display: inline-block;
}
img {
    margin-left: auto;
    margin-right: auto;
    display: block;
    max-width: v-bind('getPhotoSize()');
    max-height: v-bind('getPhotoSize()');
}

/* navigation */
.navigation {
    color: white;
    font-weight: bold;
    cursor: pointer;
}

.navigation:hover,
.navigation:focus {
    background-color: rgba(0, 0, 0, 0.8);
}

/* close button */
.close {
    position: absolute;
    top: 0px;
    right: 0px;
    font-size: 40px;
    padding: 5px 16px;
    border-radius: 0 0 0 3px;
}

/* prev/next */
.prev,
.next {
    cursor: pointer;
    position: absolute;
    top: 50%;
    width: auto;
    padding: 16px 20px;
    margin-top: -30px;
    font-size: 20px;
}
.prev {
    left: 0;
    border-radius: 0 3px 3px 0;
}
.next {
    right: 0;
    border-radius: 3px 0 0 3px;
}

/* fullscreen */
.full {
    position: absolute;
    bottom: 0px;
    right: 0px;
    font-size: 40px;
    padding: 5px 16px;
    border-radius: 3px 0 0 0;
}
</style>