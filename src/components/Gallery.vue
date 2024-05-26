<script setup>
import { ref, computed, watch } from 'vue';
import Preview from './Preview.vue'

const props = defineProps({
    albumId: {
        type: String,
        required: true
    }
})

const albumData = ref(null)

watch(props, async (_newValue, _oldValue) => {
    albumData.value = null
    const res = await fetch(import.meta.env.VITE_MEMORIA_SERVER + "/albums/" + props.albumId)
    const res_data = await res.json()
    albumData.value = {
        albumName: res_data.name,
        albumId: props.albumId,
        albumSize: res_data.num_photos,
    }
},
{immediate: true})

</script>

<template>
    <template v-if="albumData">
        <p>Album: {{ albumData.albumName }}</p>
        <Preview v-for="n in albumData.albumSize" :key="n" :album-id="albumData.albumId" :photo-id="n-1"/>
    </template>
    <template v-else>
        <p>Loading...</p>
    </template>
</template>