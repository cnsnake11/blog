
# ReactNative分享提纲

# 分享的目的

1. 了解rn是什么，特点和使用场景
2. 本次重点是学习架构（思想）而不是学习框架（用法）
3. 学习组件化的思想及其好处，目标是能运用到工作中
4. 希望随时打断进行交流讨论

# reactnative是个啥

用js写原生app的开源框架，目前官方支持ios和android。

facebook2015年发布。社区关注度非常高。版本迭代很快，基本上2周左右一个版本。

# 安卓demo包下载

[RnDemo.apk,9MB](media/RnDemo.apk)

# 优势

## 开发体验
	
1. 改代码，即时编译，即时加载
1. 兼容性好，代码可预测，各系统的模拟器、各型号的真机基本无差异
1. 组件化支撑，代码更易复用
1. flexbox布局，自适应布局更容易
1. 手势操控系统，更易开发体验佳的触屏应用
1. 支持应用内更新，自由控制发版
1. es6新特性，各种语法黑魔法
	1. 箭头函数
	1. promise
	1. 解构赋值
	1. 类定义、继承
	1. fetch
	2. 等

## 用户体验
1. 加载性能好，无白屏
2. 动画性能好，无卡顿
2. 顺畅的手势操作

# 劣势
1. 无web版，分享页等还要开发,react-web目前还不成熟
1. 无选择器的概念，定义样式很繁琐,通过开发规范部分解决
1. 尚未发布1.0

程序员首先要思考的是让用户爽，然后才是让自己爽。如果按此原则的话，劣势还是在我们接受范围内的。（用户体验和开发体验同样重要，最好是有更完美的解决方案。）

# react基本概念

## 组件化编程

### 组件化定义
组件的定义：独立的可完成某些工作的部件。可能是一个button，或者一个util类。

组件化编程的目的：复用复用复用。站在别人肩膀上可以极大的提升开发测试效率。

遇到的困难：个性化定制。通过属性、事件等扩展。

举个web组件的例子，参考此文档的前半部分：[链接](https://github.com/cnsnake11/blog/blob/master/ReactNative%E5%BC%80%E5%8F%91%E6%8C%87%E5%AF%BC/ReactNative%E7%BB%84%E4%BB%B6%E7%8A%B6%E6%80%81%E8%AE%BE%E8%AE%A1%E6%80%9D%E8%80%83.md)

### react做了什么

react是一个组件化编程的框架，提供了组件化编程的完整解决方案。[例子](http://facebook.github.io/react/)

react组件的定义。 [链接](http://facebook.github.io/react/docs/component-specs.html)

react组件生命周期及其生命周期事件。[链接](http://facebook.github.io/react/docs/component-specs.html#lifecycle-methods)

## 面向状态编程

页面中任何组件的state的改变，都会引起页面所有组件的render方法的执行。react会根据render执行的结果与当前的虚拟dom进行比对，只进行增量的更新。[链接](https://github.com/cnsnake11/blog/blob/master/ReactNative%E5%BC%80%E5%8F%91%E6%8C%87%E5%AF%BC/ReactNative%E7%BB%84%E4%BB%B6%E7%8A%B6%E6%80%81%E8%AE%BE%E8%AE%A1%E6%80%9D%E8%80%83.md#rn设计思路)

## jsx
js中的xml。[例子](http://facebook.github.io/react/)

# reactnative基本概念

## 和react的关系

1. rn使用react的组件化规范。
2. rn使用react的面向状态编程的机制。
3. rn使用react的jsx机制。
4. rn提供了一系列app开发所需要的组件。

## 常用组件
1. div View
2. span Text
3. img Image
4. input TextInput
5. button Touch*

## 样式定义
不支持选择器。不支持选择器。不支持选择器。

只能内联或者在每一个节点上声明外部css定义。所以，一般没有复用的样式使用内联，有复用的才使用外部定义。

[链接](http://reactnative.cn/docs/0.24/style.html#content)


# 开发环境

## ios

xcode+app的ios代码+js编辑器+rn开发服务器+真机or模拟器

## android

jdk+androidSdk+androidStudio+app的android代码+js编辑器+rn开发服务器+真机or模拟器

# 开发过程

1. 启动rn开发服务器
2. 启动模拟器，点开app，设置自动reload
3. 启动js代码编辑器，coding---save---看效果
4. tips:可以直接让首页显示你要开发的页面，可以快速查看效果

```
// tips用到的代码，替换index页面的最后一行
AppRegistry.registerComponent('Ask', () => require('../../../demo/demo1/animate/drag2'));
```

# 调试过程

1. console.log
2. js断点调试
3. alert
3. 页面元素检测器

# 组件设计
[链接](https://github.com/cnsnake11/blog/blob/master/ReactNative%E5%BC%80%E5%8F%91%E6%8C%87%E5%AF%BC/ReactNative%E7%9A%84%E6%9E%B6%E6%9E%84%E8%AE%BE%E8%AE%A1.md)


# 坑坑坑

1. android的textinput不能设置边框圆角
1. android的图片resizemodal=contain不好用
1. 循环依赖报错的问题
1. 安卓rn首屏白屏时间长
1. 安卓的中绝对定位元素移动出父元素会不可见，设置overflow=hidden也不好使
1. 不支持隐藏元素
1. 安卓中，使用缓存rootview，并且键盘打开情况下，直接后退出rn模块，在进入会报错
1. 魅族smartbar不占高
1. android中，设置overflow=hidden，父容器高度不能为0，至少为0.01，否则hidden无效
1. 加固后动画有掉帧现象


# 架构设计
```
业务模块
业务框架
技术框架
reactnative
android、ios操作系统
```


# 框架设计

# 版本发布

# 原生开发


