<template>
<scroll-view @scrolltolower="scrolltolower" scroll-y show-scrollbar :style="{height:props.height}">
    <slot name="list" :list="list"></slot>
    <template v-for="item,index in list" :key="index"> <slot :item="item"></slot></template>
</scroll-view>
</template>

<script setup>
import { ref } from 'vue'
const emit = defineEmits(['get'])
const page = ref(1)
const list = ref([])
const finsh = ref(false)
const props = defineProps({
    height:{ type: String, default: '100%'},
})
function scrolltolower() {
    page.value++
    emit('get',page,finsh,list)
}
function init(){
    page.value = 1
    list.value = []
    finsh.value = false
}
emit('get',page,finsh,list)
defineExpose({ init })
</script>