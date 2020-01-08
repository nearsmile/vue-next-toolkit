<template>
  <ul>
    <li>条目1</li>
    <slot name="header"/>
    <li>条目2</li>
    <slot name="body"/>
    <slot name="footer"/>
    <!-- <input type="text" v-bind="inputAttr" :list="listInput" @update:list="val => listInput = val">
    -->
  </ul>
</template>
<script>
import { ref, reactive, toRefs } from 'vue'
export default {
  setup() {
    const header = ref('header')
    const state = reactive({
      inputAttr: {
        placeholder: '测试动态属性'
      }
    })
    return {
      header,
      ...toRefs(state)
    }
  }
}
</script>