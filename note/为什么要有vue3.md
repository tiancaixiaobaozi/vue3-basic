## Vue2的难题
- 随着功能增长，复杂组件的代码难以维护
```javascript
/**
* 同分类别的功能分散
*/

export default {
  data() {
    return {
      filter: {}, // 分类数据
      pagination: {}, // 分页数据
      ...
    }
  },
  methods: {
    filterMethod: () => {}, // 分类方法
    paginationMethod: () => {}, // 分页方法
    ...
  },
  computed: {
    filterComputed() {},  // 分类的计算属性
    paginationComputed() {},  // 分页的计算属性
    ...
  },
}
```

- Mixin 缺点
```
1. 命名冲突
2. 不清楚暴露出来的变量的作用
```

- Vue2对typescript支持有限
