# PanResponder

移动设备上的手势识别要比在web上复杂得多。用户的一次触摸操作的真实意图是什么，App要经过好几个阶段才能判断。比如App需要判断用户的触摸到底是在滚动页面，还是滑动一个widget，或者只是一个单纯的点击。甚至随着持续时间的不同，这些操作还会转化。此外，还有多点同时触控的情况。

PanResponder是reactnative中实现手势系统的类。

PanResponder类可以将多点触摸操作协调成一个手势。它使得一个单点触摸可以接受更多的触摸操作，也可以用于识别简单的多点触摸手势。

它提供了一个对触摸响应系统响应器的可预测的包装。对于每一个处理函数，它在原生事件之外提供了一个新的gestureState对象。

onPanResponderMove: (event, gestureState) => {}

gestureState对象有如下的字段：

	•	stateID - 触摸状态的ID。在屏幕上有至少一个触摸点的情况下，这个ID会一直有效。
	•	moveX - 最近一次移动时的屏幕横坐标
	•	moveY - 最近一次移动时的屏幕纵坐标
	•	x0 - 当响应器产生时的屏幕坐标
	•	y0 - 当响应器产生时的屏幕坐标
	•	dx - 从触摸操作开始时的累计横向路程
	•	dy - 从触摸操作开始时的累计纵向路程
	•	vx - 当前的横向移动速度
	•	vy - 当前的纵向移动速度
	•	numberActiveTouches - 当前在屏幕上的有效触摸点的数量


示例代码:

```
componentWillMount: function() {
    this._panResponder = PanResponder.create({
      // 要求成为响应者：
      onStartShouldSetPanResponder: (evt, gestureState) => true,
      onStartShouldSetPanResponderCapture: (evt, gestureState) => true,
      onMoveShouldSetPanResponder: (evt, gestureState) => true,
      onMoveShouldSetPanResponderCapture: (evt, gestureState) => true,

      onPanResponderGrant: (evt, gestureState) => {
        // 开始手势操作。给用户一些视觉反馈，让他们知道发生了什么事情！

        // gestureState.{x,y} 现在会被设置为0
      },
      onPanResponderMove: (evt, gestureState) => {
        // 最近一次的移动距离为gestureState.move{X,Y}

        // 从成为响应者开始时的累计手势移动距离为gestureState.d{x,y}
      },
      onPanResponderTerminationRequest: (evt, gestureState) => true,
      onPanResponderRelease: (evt, gestureState) => {
        // 用户放开了所有的触摸点，且此时视图已经成为了响应者。
        // 一般来说这意味着一个手势操作已经成功完成。
      },
      onPanResponderTerminate: (evt, gestureState) => {
        // 另一个组件已经成为了新的响应者，所以当前手势将被取消。
      },
      onShouldBlockNativeResponder: (evt, gestureState) => {
        // 返回一个布尔值，决定当前组件是否应该阻止原生组件成为JS响应者
        // 默认返回true。目前暂时只支持android。
        return true;
      },
    });
  },

  render: function() {
    return (
      <View {...this._panResponder.panHandlers} />
    );
  },


```

