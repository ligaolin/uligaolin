<template>
<view class="name">
    {{ name }}
</view>

</template>

<script setup>
import { ref } from 'vue'
const name = ref('测试')
</script>

<style scoped>
.name{font-weight:bold;font-size:88rpx;}
</style>