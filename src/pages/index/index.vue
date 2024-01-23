<template>
    <!-- <l-icon name="right" />
    <l-icon name="left" />
    <l-icon name="up" />
    <l-icon name="down" />
    <l-icon name="close" /> -->
                
    <l-popup text="请选择" title="ddddd">
        <template #name>选择</template>
        <view>请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择请选择</view>
    </l-popup>
    <l-editor content="测试内容" @change="change" @upload="upload" url="https://mall.meibohate.com/api/member/file/uploads" :header="header" />
    <l-upload @change="res=>image=res" url="https://mall.meibohate.com/api/member/file/uploads" :header="header" value="path2"/>
    <l-media :item="{type:'image',path2:'https://mall.meibohate.com/storage/public/images/2023/12/28/lZVE6y9HrweKvMwqXeofFM9r51LbvgKXD9kuwtyH.jpg'}" value="path2"/>
</template>

<script setup>
import lPopup from 'uligaolin/components/l-popup/l-popup.vue'
import lEditor from 'uligaolin/components/l-editor/l-editor.vue'
import lUpload from 'uligaolin/components/l-upload/l-upload.vue'
import lMedia from 'uligaolin/components/l-media/l-media.vue'
import {ref} from 'vue'
const image = ref([])
const header = ref({ authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczpcL1wvbWFsbC5tZWlib2hhdGUuY29tXC9hcGlcL2xvZ2luXC9tZW1iZXJcL25hbWVMb2dpbiIsImlhdCI6MTcwNTY2MzAxMiwiZXhwIjoxNzEwODQ3MDEyLCJuYmYiOjE3MDU2NjMwMTIsImp0aSI6IkpXa2hUQlZ6ZDBkZElrdXQiLCJzdWIiOjE0LCJwcnYiOiI4NjY1YWU5Nzc1Y2YyNmY2YjhlNDk2Zjg2ZmE1MzZkNjhkZDcxODE4In0.iuQmdPP9Do9jgIBI8b2uftoRtBhMzhB-07IlYuJPAhw'})
function change(e) {
    console.log(e);
}
function upload(editor,res){
    console.log(editor,res.data.path2);
    if(res.code==2000) editor.insertImage({src:res.data.path2})
}
</script>

<style scoped>

</style>
