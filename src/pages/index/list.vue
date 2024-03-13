<template>
<view>
    <view>测试header</view>
    <l-list @get="getGoods" v-slot="{item}">
        <view>dfdf</view>
        <view style="height:80rpx;">{{ item }}</view>
    </l-list>
    <view>测试footer</view>
</view>
</template>

<script setup>
import { ref } from 'vue'
// import { onLoad,onShow } from '@dcloudio/uni-app'
// import { Post } from '@/api'

function getGoods(page,finsh,list) {
    console.log('加载');
    for(let i = 0;i<10;i++){
        list.value.push('测试')
    }
}
</script>

<style scoped>

</style>