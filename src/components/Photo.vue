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

</script>

<template>
    <div class="photo">
        <template v-if="imgurl">
            <img :src="imgurl"/>
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