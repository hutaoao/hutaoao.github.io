---
title: Vue3封装全局通用提示弹窗-结合el-dialog
date: 2025-08-21
description: Vue3封装全局通用提示弹窗(结合el-dialog)
tags: [Vue, Vue3, vue]
categories: [Vue, Vue3]
---
# Vue3封装全局通用提示弹窗(结合el-dialog)
const createMount = (opts: any) => {
  if (mountNode) {//确保只存在一个弹框
    document.body.removeChild(mountNode)
    mountNode = null
  }
  mountNode = document.createElement('div')
  document.body.appendChild(mountNode)
  const app: any = createApp(SubDialog, {
    ...opts,
    modeValue: true,
    remove() { //传入remove函数，组件内可移除dom 组件内通过props接收
      app.unmount(mountNode)
      document.body.removeChild(mountNode)
    }
  })
  return app.mount(mountNode)
}

function V3Popup(options: any = {}) {
  options.id = options.id || 'v3popup_' + 1 //唯一id 删除组件时用于定位
  return createMount(options)
}

V3Popup.install = (app: any) => {
  app.component('ConfirmDialog', SubDialog);// 全局注册 - 组件式
  app.config.globalProperties.$confirmDialog = V3Popup;// 全局注册 - 函数式
}
export default V3Popup
```



```vue
<!--
@component  通用提示弹窗
@date  2022.11.25

使用方法：
1）组件式
<ConfirmDialog
  title="标题"
  message="消息内容"
  v-model="testVisible"
  @confirm="testConfirm"
  @cancel="testCancel"
/>

2）函数式 - vue-class-component
import {getCurrentInstance} from 'vue';
const currentInstance = getCurrentInstance();
const {proxy: {$confirmDialog}}: any = this.currentInstance;
$confirmDialog({
  title: '标题',
  message: '消息内容',
  cancel: () => {
    console.log('取消')
  },
  confirm: () => {
    console.log('确认')
  },
});
-->

<template>
  <el-dialog
    class="confirmDialog"
    :width="width"
    :model-value="visible"
    :before-close="handleClose"
    :destroy-on-close="true"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <template #header>
{% raw %}
      <div class="my-header">{{ title }}</div>
{% endraw %}
    </template>
{% raw %}
    <div class="content">{{ message }}</div>
{% endraw %}
    <template #footer>
      <span class="dialog-footer">
        <el-button type="primary" @click="confirmFn">确认</el-button>
        <el-button @click="cancelFn">取消</el-button>
      </span>
    </template>
  </el-dialog>
</template>
<script setup lang="ts">
import {ref, defineProps, defineEmits} from "vue";

const props = defineProps({
  title: {
    type: String,
    default: "信息提示",
  },
  message: {
    type: String,
    default: "确定取消吗？",
  },
  width: {
    type: String,
    default: "400px",
  },
  // 必须为 modelValue
  modelValue: {
    type: Boolean,
    default: true,
  },
  cancel: {
    type: Function,
    default: () => {
      console.log()
    }
  },
  confirm: {
    type: Function,
    default: () => {
      console.log()
    },
  },
});
// 此处 emit 兼容组件式用法，其它为函数式用法
const emit = defineEmits(['cancel', 'confirm', 'update:modelValue']);
const visible = ref(true); // 窗体显示控制

const cancelFn = () => {
  props.cancel();
  emit('cancel');
  visible.value = false;
  emit('update:modelValue', false);
};

const confirmFn = () => {
  props.confirm();
  emit('confirm');
  visible.value = false;
  // emit('update:modelValue', false); // 关闭操作留给页面
};

const handleClose = (done: any) => {
  done();
}
</script>

<!--
自定义dialog样式
-->
<style>
.confirmDialog.el-dialog {
  border-radius: 10px;
}

.confirmDialog .el-dialog__header {
  margin: 0;
  padding: 0;
}
</style>

<style lang="scss" scoped>
.confirmDialog {
  .my-header {
    height: 50px;
    line-height: 50px;
    background: #F8F8F8;
    padding: 0 18px;
    font-size: 18px;
    color: #303133;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
  }

  .content {
    font-size: 16px;
  }
}
</style>

```





