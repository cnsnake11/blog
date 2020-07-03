# React的render优化通用解决方案

# 前言

在使用react框架(无论是单独使用react还是reactnative)的过程中，性能都是我们不得不考虑的因素，虽然react使用虚拟dom比对的方式来避免了频繁且冗余的真实dom操作，但是，虚拟dom的生成和比对并不是0损耗，仍然会耗费性能，当生成dom的逻辑过于复杂，当数据量非常大，当dom的节点非常多，都会放大这个损耗，都会使应用的帧数不足导致卡顿。

如何来降低这个损耗呢，让我们先来看一看官方提供的解决办法。

# 官方建议方案--shouldComponentUpdate

在说shouldComponentUpdate之前，先说一下setState的过程。

## setState的过程

setState是react的关键函数，对state的修改，会触发组件及其所有子组件的重绘动作。整个过程大致如下：

1. 组件A的state被改变了
1. 执行组件A的render方法。
2. 在组件A的render方法执行过程中，组件A的所有子组件也会执行render方法，子组件的子自己也会执行，以此类推，这是一个递归动作。
3. 当所有子组件、孙子组件等所有的后代组件都执行完render方法，react引擎就获得了组件A的虚拟dom。
4. react引擎使用组件A的虚拟dom和组件A的真实dom做diff
5. react引擎根据diff的结果去增量更新真实dom，这样一次setState的动作就完成了。

## shouldComponentUpdate

官方提供了shouldComponentUpdate生命周期事件来使业务代码获得控制组件是否需要执行render方法，整个setState过程变成如下：

1. 组件A的state被改变了
1. 执行组件的shouldComponentUpdate方法，如果返回false，就终止后续所有操作
1. 执行组件A的render方法。
2. 在组件A的render方法执行过程中，组件A的所有子组件也会执行render方法，子组件的子自己也会执行，以此类推，这是一个递归动作。
3. 当所有子组件、孙子组件等所有的后代组件都执行完render方法，react引擎就获得了组件A的虚拟dom。
4. react引擎使用组件A的虚拟dom和组件A的真实dom做diff
5. react引擎根据diff的结果去增量更新真实dom，这样一次setState的动作就完成了。


# 官方方案带来的问题

shouldComponentUpdate方法将react频繁的render动作转变成频繁的state和props的比对动作，但是因为shouldComponentUpdate方法会被频繁的调用，如果实现的不佳，性能也会出现问题。

那么如何优化shouldComponentUpdate方法，就成为了我们的关键路径问题.

# shouldComponentUpdate优化方案1----不可变数据类型

这是官方提供的推荐方案，确实比对的性能非常高，但是也有2个缺点，1是会创建更多的对象，占用了更多的内存，但是也还好，属于可接受范围内，2是改变了我们使用对象的原有方式和思路，习惯性的使用原有的操作对象的方法，就会产生bug，而且很难排查。

# shouldComponentUpdate优化方案2----浅比对+forceUpdate

首先设计state和props对象要尽量使用简单数据结构，也就是一层的对象结构。

然后在组件基类对象中实现默认的shouldComponentUpdate实现，并使用浅比对的方式比对state和props。

所有的组建都继承这个组件基类。

当不得不使用了复杂对象结构的时候，使用forceUpdate或者自行实现shouldComponentUpdate方法来解决。

这个方案的好处是，使用了折中的方式来解决这个问题，大部分情况使用浅比对，小部分情况使用forceUpdate；浅比对的性能虽然没有不可变数据类型好，但也不是很差，完全可以接受，而且没有引入颠倒开发思维的不可变数据类型，让出bug的几率降低，在取舍中达到一个比较令人满意的点。



[评论直达连接](https://github.com/cnsnake11/blog/issues/23)






