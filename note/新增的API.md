```
1.setup取代beforeCreate(), created()钩子, setup(props, context)
2.ref 主要负责响应简单数据类型
3.reactive 负责响应引用数据类型
4.不要在reactive对obj使用结构,否则会丧失响应性质
5.readonly() 参数可以一个响应式或者普通的对象再或者是个ref，返回一个只读代理
6.computed 需要传一个getter函数,其返回值是一个不可手动修改的ref对象
7.watch监听多个多个对象时，第一个参数为数组
8.teleport
9.emits
```
