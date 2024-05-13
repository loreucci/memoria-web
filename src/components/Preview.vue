<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps(['album_id', 'photo_id', 'preview_size'])

function getPreviewSize() {
    return (props.preview_size + 6) + "px"
}

const imgurl = ref('')

onMounted(async () => {
    const res = await fetch(import.meta.env.VITE_MEMORIA_SERVER + "/albums/" + props.album_id + "/thumbnail/" + props.photo_id + "?size=" + props.preview_size)
    imgurl.value = "data:image/jpg;base64," + (await res.json()).imgdata
})
</script>

<template>
    <div class="preview">
    <template v-if="imgurl == ''">
        <p>Loading...</p>
    </template>
    <template v-else>
        <img :src="imgurl"/>
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