<template>
  <img src="./logo.png" alt="">
  <h1 :[dynamicKey]="dynamicValue">Hello Vue 3</h1>
  <foo>
      <div class="body">{{count}}</div>
  </foo>
  <button v-bind="dynamicBind" v-on="dynamicEvent">点击量：{{ count }}次，翻倍：{{double}}</button>
</template>

<script>
import {
  ref,
  reactive,
  toRefs,
  computed,
  watch,

  onBeforeMount,
  onMounted,
  onBeforeUnmount,
  onUnmounted,
  onBeforeUpdate,
  onUpdated,
  onErrorCaptured,

} from 'vue'

import Foo from './components/foo.vue'

// 语法说明
// https://github.com/vuejs/rfcs
export default {
  components: {
    Foo
  },
  setup() {
    // 初始化数据
    const count = ref(0)
    const dynamicKey = ref('class')
    const dynamicValue = ref('dynamic-h1')
    
     // 定义方法
    const increase = () => count.value++

    const state = reactive({
      slotName: 'body',
      // 计算属性
      double: computed(() => count.value * 2 || 0),

      // 动态属性绑定
      dynamicBind: {
        style: { color: 'orange', },
        class: ['dynamic', 'btn', { 'count': computed(() => count.value % 2 === 0) }],
        'data-id': 'dynamicId_' + +new Date(),
      },
      // 动态事件绑定
      dynamicEvent: { click: increase },

      modelList: [{ name: '测试1' },{ name: '测试2' },{ name: '测试3' }],
      modelOuterInput: '外部数据',
    })

   

    // 观察器
    watch(() => console.log(`直接监听count值：${count.value}`))

    let countWatch = watch(count, (val, old, onCleanup) => {
      console.log(`countWatch：监听count值: 新值${val}，旧值${old}`)
      /* if(count.value > 4) {
        countWatch()
      } */
      onCleanup(() => {
        console.log('清除监听 countWatch')
      })
    })
    // lazy: 初始化默认不执行一次
    watch(() => state.double, (val, old) => console.log(`监听double值: 新值${val}，旧值${old}`), { lazy: true })


    // 生命周期
    onBeforeMount(() => console.log('生命周期：onBeforeMount'))
    onMounted(() => console.log('生命周期：onMounted'))

    onBeforeUnmount(() => console.log('生命周期：onBeforeUnmount'))
    onUnmounted(() => console.log('生命周期：onUnMounted'))

    onBeforeUpdate(() => console.log('生命周期：onBeforeUpdate'))
    onUpdated(() => console.log('生命周期：onUpdated'))

    onErrorCaptured((err) => console.log('生命周期：onErrorCaptured', err))


    return {
      count,
      increase,
      ...toRefs(state),
      dynamicKey,
      dynamicValue,
    }
  }
}

</script>
<style scoped>

img {
  width: 50px;
  text-align: center;
}

h1 {
  font-size: 18px;
}

button {
  outline: none;
}

.btn {
  min-width: 120px;
  height: 40px;
  border: 1px solid #ccc;
  color: #333;
  background: #fff;
  border-radius: 6px;
}

.btn.count {
  border-color: orange
}

</style>