<template>
<view class="icon" :class="'icon-'+props.name" :style="{fontSize:props.size,color:props.color}" @click="$emit('click')"></view>
</template>

<script setup>
import './iconfont.css'
const props = defineProps({
    size:{ type: [String,Number], default: '28rpx'},
    color:{ type: String, default: '#333'},
    name:{ type: String, default: ''},
})
</script>

<style scoped>
.icon {
  font-family: "iconfont" !important;
  font-size: 28rpx;
  font-style: normal;
  display: inline-block;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>