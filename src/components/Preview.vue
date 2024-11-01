<script setup>
import { ref, watch } from 'vue';

defineEmits(['previewClicked'])

const props = defineProps({
    albumId: {
        type: String,
        required: true
    },
    photoId: {
        type: Number,
        required: true
    },
    previewSize: {
        type: Number,
        default: 200,
    },
})

function getPreviewSize() {
    return props.previewSize + "px"
}

function padNumber(v) {
    return v.toString().padStart(2, "0")
}

const imgurl = ref(null)
const timestamp = ref(null)
const showTimestamp = ref(false)

watch(
    props,
    async (_newValue, _oldValue) => {
        const res = await fetch(import.meta.env.VITE_MEMORIA_SERVER + "/albums/" + props.albumId + "/thumbnail/" + props.photoId + "?size=" + props.previewSize)
        const photo = await res.json()
        imgurl.value = "data:image/jpg;base64," + photo.imgdata
        const d = new Date(photo.timestamp*1000)
        timestamp.value = `${padNumber(d.getUTCDate())}/${padNumber(d.getUTCMonth()+1)}/${d.getUTCFullYear()} ${padNumber(d.getUTCHours())}:${padNumber(d.getUTCMinutes())}:${padNumber(d.getUTCSeconds())}`
    },
    {immediate: true}
)

</script>

<template>
    <div class="preview" @click="$emit('previewClicked', props.photoId)" @mouseenter="showTimestamp=true" @mouseleave="showTimestamp=false">
        <template v-if="imgurl">
            <template v-if="showTimestamp">
                <div class="timestamp">{{ timestamp }}</div>
            </template>
            <img :src="imgurl"/>
        </template>
        <template v-else>
            <p>Loading...</p>
        </template>
    </div>
</template>

<style scoped>
div.preview {
    margin: 5px;
    padding: 2px;
    width: v-bind('getPreviewSize()');
    height: v-bind('getPreviewSize()');
    border: solid 1px;
    align-content: center;
    position: relative;
}
img {
    margin-left: auto;
    margin-right: auto;
    display: block;
}
.timestamp {
    position: absolute;
    bottom: 2px;
    left: 2px;
    background-color: black;
    color: white;
}
</style>