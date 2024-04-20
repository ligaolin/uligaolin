<template>
<l-popup ref="pop" :height="props.height" :index="props.index">
    <template #name>
        <slot name="text" :html="html">
            <view :style="props.styles" style="color:#333;display:flex;font-size:28rpx;">
                <view style="margin-right:6rpx;">{{html?'编辑':'添加'}}</view>
                <l-icon name="down" color="#333" size="26rpx"></l-icon>
            </view>
        </slot>
    </template>
    <sp-editor 
        ref="sp"
        :placeholder="placeholder"
        :readOnly="readOnly"
        :maxlength="maxlength"
        :templates="templates"
        @init="initEditor"
        @input="e=>$emit('change',e)" 
        @upinImage="upinImage"
        @overMax="e=>$emit('overMax',e)"
        @submit="e=>{
            html = e
            $emit('submit',e)
            pop.close()
        }"
    />
</l-popup>
</template>

<script setup>
import { ref,watch } from 'vue'
import lPopup from '../l-popup/l-popup.vue'
import lIcon from '../l-icon/l-icon.vue'
import spEditor from './sp-editor/sp-editor.vue'
const sp = ref(null)
const pop = ref(null)
const html = ref('')
const emit = defineEmits(['upload','change','submit'])
const props = defineProps({
    placeholder: { type: String, default: '请输入内容' },
    // 是否只读
    readOnly: { type: Boolean, default: false },
    // 最大字数限制，-1不限
    maxlength: { type: [Number,String], default: -1 },
    // 初始模板
    templates: { type: String, default: '' },
    // 设置内容
    content: { type: String, default: '' },
    // 上传地址
    url: { type: String, default: '' },
    // 上传请求头信息
    header: { type: Object, default: {} },
    // 弹出z-index
    index: { type: [String,Number], default: 1000 },
    // 高度
    height: { type: [String,Number], default: '92%' },
})

/**
 * 编辑器就绪
 * @param {Object} editor 编辑器实例，你可以自定义调用editor实例的方法
 */
function initEditor(editor) {
    // 初始化编辑器内容
    html.value = props.content
    editor.setContents({ html: props.content })
}
watch(()=>props.content,()=>{
    if(sp.value && sp.value.editorCtx) {
        html.value = props.content
        sp.value.editorCtx.setContents({ html: props.content })
    }
},{deep:true})


/**
 * @param {Object} tempFiles
 * @param {Object} editorCtx
 */
function upinImage(tempFiles, editorCtx) {
    console.log(tempFiles);
    uni.uploadFile({
        url: props.url,
        filePath: tempFiles[0].path,
        name: 'file',
        header: props.header,
        success: (res) => {
            let data = JSON.parse(res.data)
            emit('upload',editorCtx,data)
        }
    });
}
</script>