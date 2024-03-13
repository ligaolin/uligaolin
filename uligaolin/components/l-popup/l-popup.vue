<template>
    <view @click="show=true">
        <slot name="name" :text="props.text"> {{ props.text }} </slot>
    </view>
    <view class="popup" v-if="show" @click="close" :style="{zIndex:props.index}">
        <view class="body" @click.stop :style="{height:props.height,padding:props.padding,borderRadius:radius,background:background}">
            <view class="top" v-if="props.close||props.title">
                <view></view>
                <view class="title">{{props.title}}</view>
                <l-icon class="close" name="close" @click="close" />
            </view>
            <slot></slot>
        </view>
    </view>
</template>

<script setup>
import { ref } from 'vue'
import lIcon from '../l-icon/l-icon.vue'
const show = ref(false)
const open = ()=>{ show.value = true }
const close = ()=>{ show.value = false }

const props = defineProps({
    height: { type: String, default: '80vh' },
    padding: { type: String, default: '30rpx' },
    radius: { type: String, default: '40rpx 40rpx 0 0' },
    background: { type: String, default: 'white' },

    close: { type: Boolean, default: true },
    title: { type: String, default: '' },
    text: { type: String, default: '' },
    index: { type: [String,Number], default: 1000 },
})
defineExpose({ open,close })
</script>

<style scoped>
.popup{background: rgba(0,0,0,0.5); bottom: 0; position: fixed; width: 100vh; height: 100vh;left: 0;}
.body{width:100%;height:100%;box-sizing:border-box;position:fixed;bottom:0;left: 0;}

.top{align-items: center; display: flex; justify-content:space-between;margin-bottom:20rpx;}
.title{font-weight:bold;font-size:32rpx;}
</style>