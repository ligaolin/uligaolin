<template>
<image v-if="item.type=='image'" :src="item[props.value]?item[props.value]:item[props.value2]" :style="{width:width,height:height,borderRadius:radius}" :mode="mode"></image>
<video v-else-if="item.type=='video'" @click.self.stop :src="item[props.value]?item[props.value]:item[props.value2]" show-mute-btn :style="{width:width,height:height,borderRadius:radius}"></video>
<view v-else :style="{width:width,height:height,borderRadius:radius}">{{ item[props.value]?item[props.value]:item[props.value2] }}</view>
</template>

<script setup>
import { ref,watch } from 'vue'
const item = ref({ type:'', })
const props = defineProps({
    item: { type: [Object,Array], default: [] }, 
    width: { type: String, default: '100%' }, 
    height: { type: String, default: '' }, 
    radius: { type: String, default: '' }, 
    mode: { type: String, default: 'aspectFill' }, 

    value: { type: String, default: 'path' }, 
    value2: { type: String, default: 'url' }, 
})
const setData = ()=>{
    if(props.item){
        if(Array.isArray(props.item) && props.item.length){
            item.value = props.item[0]
        }else if(typeof props.item == 'object'){
            item.value = props.item
        }
    }
}
setData()
watch(()=>props.item,()=>{
    setData()
})
</script>

<style scoped>

</style>