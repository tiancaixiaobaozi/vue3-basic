<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <h1>{{ count }}</h1>
  <h1>{{ double }}</h1>
  <ul>
    <li v-for="num in numbers" :key="num">{{ num }}</li>
  </ul>
  <h2>{{ person.name }}</h2>
  <button @click="add">点赞</button>
  <button @click="updateGreetings">更新标题</button>
  <h1>x:{{ x }}, y: {{ y }}</h1>
  <p>错误信息：{{ error }}</p>
  <Suspense>
    <template #default>
      <div>
        <async-show />
        <dog-show />
      </div>
    </template>
    <template #fallback>
      <h2>加载中...</h2>
    </template>
  </Suspense>
  <h1 v-if="loading">Loading...</h1>
  <img v-if="loaded" :src="result.message" />
  <button @click="openModal">打开modal</button>
  <modal
    :is-open="modalIsOpen"
    @close-modal="onModalClose"
  >my modal...</modal>
</template>

<script lang="ts">
import {
  ref,  // 定义响应变量
  computed,
  reactive,
  toRefs,
  onMounted,
  onUpdated,
  onRenderTriggered,
  onUnmounted,
  watch,
  defineComponent,
  onErrorCaptured,
} from 'vue';
import useMousePosition from "@/hooks/useMousePosition";
import useURLLoader from "@/hooks/useURLLoader";
import Modal from "@/components/Modal.vue";
import AsyncShow from "@/components/AsyncShow.vue";
import DogShow from "@/components/DogShow.vue";

interface DataProps {
  count: number;
  double: number;
  add: () => void;
  numbers: number[];
  person: { name?: string };
}

interface DogResult {
  message: string;
  status: string;
}

export default defineComponent({
  name: 'App',
  components: { Modal, AsyncShow, DogShow },
  setup() {
    // setup中不能访问this,放在生命周期之前
    // 1. 基本使用
    // const count = ref(0)
    // const add = () => {
    //   count.value++ // 值存在于value上
    // }
    // const double = computed(() => {
    //   return count.value * 2
    // })
    // 2. reactive
    const data: DataProps = reactive({
      count: 0,
      add: () => { data.count++ },  // reactive中，不需要用变量的value
      double: computed(() => data.count * 2),
      numbers: [0, 1, 2],
      person: {}
    })
    const refData = toRefs(data)
    data.numbers[0] = 5
    data.person.name = '张三'
    // 3.生命周期
    // onMounted(() => {
    //   console.log('mounted...')
    // })
    // onUpdated(() => {
    //   console.log('update...')
    // })
    // onRenderTriggered((event) => {
    //   // 更新时触发
    //   console.log(event)
    // })
    // 4.副作用
    const greetings = ref('')
    const updateGreetings = () => {
      greetings.value += 'Hello!'
    }
    // watch监听多个多个对象时，第一个参数为数组
    // 不可直接watch data.count, 需监听其getter
    watch([greetings, () => data.count], (newVal, oldValue) => {
      console.log(newVal, oldValue)
      document.title = 'updated' + greetings.value + data.count
    })
    // 5.调用自定义hooks
    const { x, y } = useMousePosition()
    const { result, loading, loaded } = useURLLoader<DogResult>('https://dog.ceo/api/breeds/image/random')
    watch(result, () => {
      if (result.value) {
        console.log('value:', result.value.message)
      }
    })
    // 6.teleport
    const modalIsOpen = ref(false)
    const openModal = () => {
      modalIsOpen.value = true
    }
    const onModalClose = () => {
      modalIsOpen.value = false
    }
    // 7.error
    const error = ref(null)
    onErrorCaptured((e: any) => {
      error.value = e
      return true // 表示错误是否向上传播
    })

    return {
      // count,
      // add,
      // double,
      // data,
      /* 失去响应 */
      // ...data
      // count: data.count,
      // double: data.double,
      // add: data.add,
      ...refData,
      updateGreetings,
      x,
      y,
      result,
      loading,
      loaded,
      modalIsOpen,
      openModal,
      onModalClose,
      error,
    }
  },
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
