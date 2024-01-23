<template>

<template v-for="item,index in fileList" :key="index">
    <view class="mediaBox" v-if="item.type=='image'||item.type=='video'" :style="{width:width,height:height}">
        <lMedia :item="item" :width="width" :height="height" :value="props.value" />
        <view class="icon" @click="delFile(index)">
            <l-icon name="close_cricle" color="#666" size="36rpx"></l-icon>
        </view>
    </view>
    <view v-else class="other">
        <view>{{ item.name }}</view>
        <view class="icon" @click="delFile(index)">
            <l-icon name="close_cricle" color="#666" size="36rpx"></l-icon>
        </view>
    </view>
</template>
<view @click="upload" class="btn" :style="{width:width,height:height}">
    <l-icon name="image" color="#888" size="60rpx"></l-icon>
</view>
</template>

<script setup>
import { ref,watch } from 'vue'
import lIcon from '../l-icon/l-icon.vue'
import lMedia from '../l-media/l-media.vue'
const emit = defineEmits(['change'])
const props = defineProps({
    type: { type: [String,Array], default: ['image'] },
    sourceType: { type: [String,Array], default: ['album', 'camera'] },
    count: { type: [String,Number], default: 1 }, // 可上传数量
    list: { type: [Object,Array], default: [] },
    width: { type: [String,Number], default: '140rpx' },
    height: { type: [String,Number], default: '140rpx' },

    // 上传地址
    url: { type: String, default: '' },
    // 上传请求头信息
    header: { type: Object, default: {} },
    value: { type: String, default: 'path' }, 
})
const fileList = ref([])
fileList.value = JSON.parse(JSON.stringify(props.list))
watch(()=>props.list,()=>{
    fileList.value = JSON.parse(JSON.stringify(props.list))
})
function delFile(index){
    uni.showModal({content:'确定删除吗',success(res) {
		if (res.confirm) {
            fileList.value.splice(index,1)
            emit('change',fileList.value)
        }
	}})
}
function upload(){
    uni.chooseMedia({
        mediaType: props.type,
        sourceType: props.sourceType,
        success (res) {
            if(res.errMsg=='chooseMedia:ok'){
                if(parseInt(props.count)!=1 && fileList.value.length + res.tempFiles.length > parseInt(props.count)){
                    uni.showToast({ title: '上传数量限制：'+props.count, icon: "none" });return;
                }else{
                    for(let i in res.tempFiles){
                        uni.uploadFile({
                            url: props.url,
                            filePath: res.tempFiles[i].tempFilePath,
                            name: 'file',
                            header: props.header,
                            success: (uploadFileRes) => {
                                if(uploadFileRes.data){
                                    let res1 = JSON.parse(uploadFileRes.data)
                                    uni.showToast({ title:res1.msg, icon: "none" });
                                    if(res1.code==2000) {
                                        if(parseInt(props.count)==1) {
                                            fileList.value = [res1.data]
                                        }else {
                                            fileList.value.push(res1.data)
                                        }
                                        emit('change',fileList.value)
                                    }
                                }
                            }
                        });
                    }
                }
            }else{
                uni.showToast({ title: res.errMsg, icon: "none" });return;
            }
        }
    })
}
</script>

<style scoped>
.mediaBox{position:relative;margin-bottom:20rpx;display:inline-block;margin-right:20rpx;}
.mediaBox .icon{display:flex;align-items:center;justify-content:center;position:absolute;top:-16rpx;right:-16rpx;border-radius:50rpx;background:white;}

.other{width:100%;word-break: break-all;color:#666;display:flex;justify-content: space-between;align-items:center;margin-bottom:20rpx;}
.other .icon{margin-left:20rpx;}

.btn{background:#f5f5f5;display:inline-flex;align-items:center;justify-content:center;vertical-align:top;}
</style>