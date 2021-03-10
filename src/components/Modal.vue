<template>
  <teleport to="#modal">
    <div v-if="isOpen" id="center">
      <h2>
        <slot>this is modal</slot>
      </h2>
      <button @click="buttonClick">关闭</button>
    </div>
  </teleport>
</template>

<script lang="ts">
  import { defineComponent } from 'vue'

  export default defineComponent({
    name: "Modal",
    props: {
      isOpen: Boolean
    },
    // 事件验证
    emits: {
      'close-modal': null
    },
    setup(props, context) {
      const buttonClick = () => {
        context.emit('close-modal')
      }
      return {
        buttonClick
      }
    }
  })
</script>

<style>
 #center {
   width: 200px;
   height: 200px;
   border: 2px solid #000;
   background: #999999;
   position: fixed;
   left: 50%;
   top: 50%;
   transform: translate(-50%, -50%);
 }
</style>
