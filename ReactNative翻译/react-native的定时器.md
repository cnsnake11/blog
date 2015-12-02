# 定时器 Timers

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
原生app流畅的一大原因就是利用多线程在交互和动画的线程中避免同时执行大量计算。在ReactNative中，会受到js只有一个执行线程的限制，但是你可以使用交互管
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

The touch handling system considers one or more active touches to be an 'interaction' and will delay runAfterInteractions() callbacks until all touches have ended or been cancelled.

InteractionManager also allows applications to register animations by creating an interaction 'handle' on animation start, and clearing it upon completion:

```
var handle = InteractionManager.createInteractionHandle();
// run animation... (`runAfterInteractions` tasks are queued)
// later, on animation completion:
InteractionManager.clearInteractionHandle(handle);
// queued tasks run if all handles were cleared
```

###TimerMixin 
We found out that the primary cause of fatals in apps created with React Native was due to timers firing after a component was unmounted. To solve this recurring issue, we introduced TimerMixin. If you include TimerMixin, then you can replace your calls to setTimeout(fn, 500) with this.setTimeout(fn, 500) (just prepend this.) and everything will be properly cleaned up for you when the component unmounts.

This library does not ship with React Native - in order to use it on your project, you will need to install it with npm i react-timer-mixin --save from your project directory.

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

We strongly discourage using the global setTimeout(...) and recommend instead that you use this.setTimeout(...) provided by react-timer-mixin. This will eliminate a lot of hard work tracking down bugs, such as crashes caused by timeouts firing after a component has been unmounted.
# 定时器 Timers

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
原生app流畅的一大原因就是利用多线程在交互和动画的线程中避免同时执行大量计算。在ReactNative中，会受到js只有一个执行线程的限制，但是你可以使用交互管
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

The touch handling system considers one or more active touches to be an 'interaction' and will delay runAfterInteractions() callbacks until all touches have ended or been cancelled.

InteractionManager also allows applications to register animations by creating an interaction 'handle' on animation start, and clearing it upon completion:

```
var handle = InteractionManager.createInteractionHandle();
// run animation... (`runAfterInteractions` tasks are queued)
// later, on animation completion:
InteractionManager.clearInteractionHandle(handle);
// queued tasks run if all handles were cleared
```

###TimerMixin 
We found out that the primary cause of fatals in apps created with React Native was due to timers firing after a component was unmounted. To solve this recurring issue, we introduced TimerMixin. If you include TimerMixin, then you can replace your calls to setTimeout(fn, 500) with this.setTimeout(fn, 500) (just prepend this.) and everything will be properly cleaned up for you when the component unmounts.

This library does not ship with React Native - in order to use it on your project, you will need to install it with npm i react-timer-mixin --save from your project directory.

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

We strongly discourage using the global setTimeout(...) and recommend instead that you use this.setTimeout(...) provided by react-timer-mixin. This will eliminate a lot of hard work tracking down bugs, such as crashes caused by timeouts firing after a component has been unmounted.
