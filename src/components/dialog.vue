<!-- Dialog.vue -->

<template>
    <div class="dialog" v-if="visible">
      <div class="dialog-content">
        <slot></slot> <!-- 插槽，用于显示对话框内容 -->
        <button @click="closeDialog">關閉</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      visible: Boolean // 用于控制对话框的可见状态
    },
    methods: {
      closeDialog() {
        this.$emit('close'); // 触发自定义事件，通知父组件关闭对话框
      }
    }
  };
  </script>
  
  <style scoped>
  .dialog {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .dialog-content {
    background-color: white;
    padding: 20px;
    border-radius: 5px;
  }
  </style>
  