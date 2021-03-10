<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>num:{{ num }}</p>
    <div>
      <p>name:{{ obj.name }}</p>
      <p>age:{{ obj.age }}</p>
      <button @click="obj.age++">增加</button>
    </div>
    <p>obj2-name:{{ obj2.name }}</p>
    <p>num2: {{ num2 }}</p>
    <div class="ref-dom" ref="test"></div>
  </div>
</template>

<script>
import {
  ref,
  reactive,
  readonly,
  computed,
  onMounted,
} from 'vue'

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  // 不要在这里对props解构，否则导致其丧失响应式
  // setup取代beforeCreate(), created()钩子
  setup(props, context) {
    console.log(props)
    console.log(context)  // attrs,emit,slots
    // 1.ref 主要负责响应简单数据类型
    const num = ref(1)
    // 2.reactive 负责响应引用数据类型
    // 不要在reactive对使用obj结构,否则会丧失响应性质
    const obj = reactive({
      name: '张三',
      age: 18
    })
    // 3.readonly() 参数可以一个响应式或者普通的对象再或者是个ref，返回一个只读代理
    const obj2 = readonly({ name: '李四' })
    obj2.name = '王五'  // Warning: Set operation on key "name" failed: target is readonly.
    // 4.computed 需要传一个getter函数,其返回值是一个不可手动修改的ref对象
    const num2 = computed(() => num.value * 100)
    // 5.emit派发事件
    context.emit('test', 'hello world')
    // 6.获取test的refs
    const test = ref(null)
    onMounted(() => {
      console.log(test.value)
    });

    return { num, obj, obj2, num2, test }
  }
}
</script>
