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
            <span class="navigation close" @click="$emit('closePhoto')"><font-awesome-icon :icon="['fas', 'xmark']"/></span>
            <span class="navigation full" @click="goFullscreen()"><font-awesome-icon :icon="['fas', 'expand']"/></span>
            <template v-if="imgurl">
                <img :src="imgurl" ref="photoimg"/>
            </template>
            <template v-else>
                <p>Loading...</p>
            </template>
            <a class="navigation prev" @click="$emit('previousPhoto')"><font-awesome-icon :icon="['fas', 'chevron-left']"/></a>
            <a class="navigation next" @click="$emit('nextPhoto')"><font-awesome-icon :icon="['fas', 'chevron-right']"/></a>
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
    margin: auto;
    padding: 0;
    max-width: 90vw;
    max-height: 90vh;
    display: inline-block;
}
img {
    margin-left: auto;
    margin-right: auto;
    display: block;
    max-width: 90vw;
    max-height: 90vh;
}

/* navigation */
.navigation {
    color: white;
    font-weight: bold;
    cursor: pointer;
    font-size: 40px;
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