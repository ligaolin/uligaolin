<template>
<scroll-view @scrolltolower="scrolltolower" scroll-y show-scrollbar :style="{height:props.height}">
    <slot name="header" :list="list"></slot>
    <template v-for="item,index in list" :key="index"> <slot :item="item" :index="index"></slot></template>
    <uni-load-more v-if="load" :iconSize="iconSize" :showIcon="showIcon" :showText="showText" :iconType="iconType" :color="color" :contentText="contentText" :status="status"></uni-load-more>
    <slot name="footer" :list="list"></slot>
</scroll-view>
</template>

<script setup>
import { ref,watch } from 'vue'
import { onMounted } from 'vue'
import { onReachBottom } from '@dcloudio/uni-app'
const emit = defineEmits(['get'])
const status = ref('more') // 加载状态
const page = ref(1)
const list = ref([])
const finsh = ref(false)
const props = defineProps({
    height:{ type: String, default: '100%'},
    auto:{ type: Boolean, default: true},
    onReachBottom:{ type: Boolean, default: true}, // 触发下一页方式

    load:{ type: Boolean, default: true}, // 是否使用加载提示
    iconSize:{ type: Number, default: 24}, // 加载指定图标大小
    showIcon:{ type: Boolean, default: true}, // 加载是否显示 loading 图标
    showText:{ type: Boolean, default: true}, // 加载**[1.3.3新增]**是否显示文本
    iconType:{ type: String, default: 'auto'}, // 加载指定图标样式
    color:{ type: String, default: '#777777'}, // 加载图标和文字颜色
    contentText:{ type: Object, default: {contentdown: "上拉显示更多",contentrefresh: "正在加载...",contentnomore: "没有更多数据了"}}, // 加载各状态文字说明
})
watch(finsh,(newData)=>{
    if(newData) status.value = 'no-more'
})
function get(){
    if(!finsh.value) status.value = 'loading'
    emit('get',page,finsh,list)
}
function nextPage() {
    page.value++
    get()
}
function scrolltolower() {
    if(!props.onReachBottom) nextPage()
}
onReachBottom(()=>{
    if(props.onReachBottom) nextPage()
})
function init(){
    page.value = 1
    list.value = []
    finsh.value = false
    get()
}
onMounted(()=>{
    if(props.auto) get()
})
defineExpose({ init })
</script>