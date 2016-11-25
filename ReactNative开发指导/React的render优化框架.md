# React的render优化通用解决方案

# 前言

在使用react框架(无论是单独使用react还是reactnative)的过程中，性能都是我们不得不考虑的因素，虽然react使用虚拟dom比对的方式来避免了频繁且冗余的真实dom操作，但是，虚拟dom的生成和比对并不是0损耗，仍然会耗费性能，当生成dom的逻辑过于复杂，当数据量非常大，当dom的节点非常多，都会放大这个损耗，都会使应用的帧数不足导致卡顿。

如何来降低这个损耗呢，让我们先来看一看官方提供的解决办法。

# 官方建议方案--shouldComponentUpdate

在说shouldComponentUpdate之前，先说一下setState的过程。

## setState的过程

setState是react的关键函数，对state的修改，会触发组件及其所有子组件的重绘动作。整个过程大致如下：

1. 组件A的state被改变了，执行组件A的render方法。
2. 在组件A的render方法执行过程中，组件A的所有子组件也会执行render方法，子组件的子自己也会执行，以此类推，这是一个递归动作。
3. 当所有子组件、孙子组件等所有的后代组件都执行完render方法，react引擎就获得了组件A的虚拟dom。
4. react引擎使用组件A的虚拟dom和组件A的真实dom做diff
5. react引擎

render之前的是否需要render的事件接口。

# 官方方案带来的问题

因为shouldComponentUpdate方法会被频繁的调用，

浅比对的局限性。

针对这个问题的解法，使用不可变数据类型。不可变数据类型带来的问题。

# 究竟如何解决

分而治之。能浅比对就浅比对【使用条件参考下面】；不能浅比对，使用强制刷新forceUpdate的方式or重新实现shouldComponentUpdate方法。

框架提供基础对象。

基础对象实现默认的生命周期函数，进行浅比对。

使用条件：pure组件；无复杂类型的state；

# 特殊情况处理 

不是pure组件。

state or props是复杂数据类型，浅比对不生效。

# 总结



