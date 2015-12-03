# 定时器 Timers -未完

原文地址：http://facebook.github.io/react-native/docs/timers.html

定时器是应用程序的重要组件，ReactNative实现了浏览器的定时器。

###定时器
1. setTimeout, clearTimeout
1. setInterval, clearInterval
1. setImmediate, clearImmediate
1. requestAnimationFrame, cancelAnimationFrame

requestAnimationFrame(fn) 与setTimeout（fn，0）并不一样，前面的是会等待当前的所有帧结束之后才触发，后面的是会尽快的被触发（over 1000x per second on a iPhone 5S 这句不会翻）。

setImmediate会在当前js执行块的最后立刻被执行，在与native通信之前。如果你使用这个方法，它会被立刻执行，不会被任何代码所有阻断包括native代码。

Promise的实现就是基于setImmediate作为基础的。

###InteractionManager交互管理器
原生app流畅的一大原因就是利用多线程在交互和动画的线程中避免同时执行大量计 算。在ReactNative中，会受到js只有一个执行线程的限制，但是你可以使用交互管
理器来保证耗时复杂工作在交互动画完成之后才开始执行。

示例代码：

```
InteractionManager.runAfterInteractions(() => {
   // ...long-running synchronous task...
});
```

几个定时器的比较：

1. requestAnimationFrame(): 在一个动画效果完成之后执行代码。
1. setImmediate/setTimeout/setInterval():稍后执行代码，这些方法可能会拖慢动画效果。
1. runAfterInteractions():稍后执行代码，不会拖慢当前的动画效果。

触摸系统会把一组触摸事件作为一次交互，runAfterInteractions()注册的回调函数会在当前的所有交互结束后触发。

你也可以使用交互管理器在自己的应用程序中，在动画的开始创建一个handle，在动画结束的时候清除掉它。

```
var handle = InteractionManager.createInteractionHandle();

//执行动画。。。。runAfterInteractions注册的函数会在稍后动画结束后触发。

InteractionManager.clearInteractionHandle(handle)

//所有的handle都clear之后就会触发注册的事件了

```

###TimerMixin 

用RN构建的app，发生系统崩溃通常都是因为定时任务在组件已经被卸载之后触发造成的。解决这个问题最好的办法就是使用TimerMixin。如果你引入了TimerMixin，你就可以使用this.setTimeout代替setTimeout，这个方法可以保证当组件被卸载时所有的定时任务都被正确的清除了。

RN中并没有带这个库，你可以在你的工程里用命令npm i react-timer-mixin --save来安装。

示例代码：

```
var TimerMixin = require('react-timer-mixin');

var Component = React.createClass({
  mixins: [TimerMixin],
  componentDidMount: function() {
    this.setTimeout(
      () => { console.log('I do not leak!'); },
      500
    );
  }
});

```

我们特别不建议使用原生的全局方法setTimeOut，强烈建议使用react-timer-mixin提供的this。setTimeout。这会消除很多隐性的bug，比如说在组件卸载后执行定时任务引起的app崩溃。





# 定时器 Timers -未完

原文地址：http://facebook.github.io/react-native/docs/timers.html

定时器是应用程序的重要组件，ReactNative实现了浏览器的定时器。

###定时器
1. setTimeout, clearTimeout
1. setInterval, clearInterval
1. setImmediate, clearImmediate
1. requestAnimationFrame, cancelAnimationFrame

requestAnimationFrame(fn) 与setTimeout（fn，0）并不一样，前面的是会等待当前的所有帧结束之后才触发，后面的是会尽快的被触发（over 1000x per second on a iPhone 5S 这句不会翻）。

setImmediate会在当前js执行块的最后立刻被执行，在与native通信之前。如果你使用这个方法，它会被立刻执行，不会被任何代码所有阻断包括native代码。

Promise的实现就是基于setImmediate作为基础的。

###InteractionManager交互管理器
原生app流畅的一大原因就是利用多线程在交互和动画的线程中避免同时执行大量计 算。在ReactNative中，会受到js只有一个执行线程的限制，但是你可以使用交互管
理器来保证耗时复杂工作在交互动画完成之后才开始执行。

示例代码：

```
InteractionManager.runAfterInteractions(() => {
   // ...long-running synchronous task...
});
```

几个定时器的比较：

1. requestAnimationFrame(): 在一个动画效果完成之后执行代码。
1. setImmediate/setTimeout/setInterval():稍后执行代码，这些方法可能会拖慢动画效果。
1. runAfterInteractions():稍后执行代码，不会拖慢当前的动画效果。

触摸系统会把一组触摸事件作为一次交互，runAfterInteractions()注册的回调函数会在当前的所有交互结束后触发。

你也可以使用交互管理器在自己的应用程序中，在动画的开始创建一个handle，在动画结束的时候清除掉它。

```
var handle = InteractionManager.createInteractionHandle();

//执行动画。。。。runAfterInteractions注册的函数会在稍后动画结束后触发。

InteractionManager.clearInteractionHandle(handle)

//所有的handle都clear之后就会触发注册的事件了

```

###TimerMixin 

用RN构建的app，发生系统崩溃通常都是因为定时任务在组件已经被卸载之后触发造成的。解决这个问题最好的办法就是使用TimerMixin。如果你引入了TimerMixin，你就可以使用this.setTimeout代替setTimeout，这个方法可以保证当组件被卸载时所有的定时任务都被正确的清除了。

RN中并没有带这个库，你可以在你的工程里用命令npm i react-timer-mixin --save来安装。

示例代码：

```
var TimerMixin = require('react-timer-mixin');

var Component = React.createClass({
  mixins: [TimerMixin],
  componentDidMount: function() {
    this.setTimeout(
      () => { console.log('I do not leak!'); },
      500
    );
  }
});

```

我们特别不建议使用原生的全局方法setTimeOut，强烈建议使用react-timer-mixin提供的this。setTimeout。这会消除很多隐性的bug，比如说在组件卸载后执行定时任务引起的app崩溃。






