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

const imgurl = ref(null)

watch(
    props,
    async (_newValue, _oldValue) => {
        const res = await fetch(import.meta.env.VITE_MEMORIA_SERVER + "/albums/" + props.albumId + "/thumbnail/" + props.photoId + "?size=" + props.previewSize)
        imgurl.value = "data:image/jpg;base64," + (await res.json()).imgdata
    },
    {immediate: true}
)

</script>

<template>
    <div class="preview" @click="$emit('previewClicked', props.photoId)">
        <template v-if="imgurl">
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
}
img {
    margin-left: auto;
    margin-right: auto;
    display: block;
}
</style>