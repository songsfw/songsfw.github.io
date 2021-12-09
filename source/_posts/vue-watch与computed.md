---
title: vue watch与computed
date: 2021-12-08 16:17:02
categories:
- 前端开发
tags: 
- vue
cover: /gallery/covers/vector_landscape_1.svg
thumbnail: /gallery/covers/vector_landscape_1.svg
toc: true
---

watch和computed都是监听属性变化的，在某些场景下功能是一致的，如 需要监听一个数据的改变引起其他数据的变化

但是watch 和 computed侧重点不同

<!-- more -->

### computed

computed 计算属性，可以类比成data里的属性，他设计的初衷就是处理复杂逻辑数据的展示

```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```
```js
computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
```

这里和 methods 做个对比

methods同样可以实现相同的效果，

```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage() }}"</p>
</div>
```
```js
methods: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
```

但是这不是说我们可以混用methods和computed，methods就是单纯的方法，需要的时候调用

computed就厉害了，一个当然是他可以监听数据改变

官方解释:你可以像绑定普通 property 一样在模板中绑定计算属性。Vue 知道 vm.reversedMessage 依赖于 vm.message，因此当 vm.message 发生改变时，所有依赖 vm.reversedMessage 的绑定也会更新

另一个是，computed是基于它们的响应式依赖进行缓存的，只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。

- computed 相比 更适用于大量数据计算 并需要缓存数据的场景

### watch

watch 也是监听属性变化，不同的是watch监听已经挂载到vm上的数据，所以 除了监听data的变化，还可以用watch监听computed里计算属性的变化，以及props的变化

官方解释:当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

### 总结一下区别

1 computed是计算一个新的属性，并将该属性挂载到vm（Vue实例）上，而watch是监听已经存在且已挂载到vm上的数据，所以用watch同样可以监听computed计算属性的变化（其它还有data、props） 

2 computed本质是一个惰性求值的观察者，具有缓存性，只有当依赖变化后，第一次访问 computed 属性，才会计算新的值，而watch则是当数据发生变化便会调用执行函数

3 从使用场景上说，computed适用多个数据影响一个数据被，而watch适用一个数据影响多个数据；







