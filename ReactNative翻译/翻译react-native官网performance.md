# 翻译react-native官网performance

原文地址： http://facebook.github.io/react-native/docs/performance.html

使用react native 代替webview技术来构建app最大的理由就是60FPS流畅的体验。我们尽可能的让开发者可以专注于app开发而不是性能优化，但是目前RN还不能完全的达到此目标，并不能自动选择最优的方式，所以，还需要开发者关注并进行调整。

本篇文章介绍了性能调优的基本技巧，同时讨论一些常见问题及其解决方案。
## 帧数
动画效果其实就是静态图片的连续播放，每秒图片播放的越多，动画就越平滑流畅。60FPS的意思就是1秒有60张图片进行了播放，一张图叫一帧，如果想达到60FPS，需要16.67ms就要播放一帧，所以就要求程序在16.67ms就要生成一帧，如果超过这个时间，就会掉帧，就会觉得程序不流畅。

打开开发者菜单，点击 show fps monitor 会有2个帧数显示在屏幕上。
### Javascript frame rate【js帧数】
大多数情况下，你的业务逻辑会运行在js线程下。这个线程处理了api调用，触摸事件，等等...对原生view的更新是批量提交的，也是尽力保持在每帧之前提交。如果js线程在下一帧来到之前不能进行提交，就会掉帧。举个例子，如果你在一个复杂的组件的根节点调用`this.setState`方法，就会导致一次复杂的diff计算，如果这次计算耗时200ms就相当于掉了12帧。在这过程中，所有js控制的动画都会卡住。事实上只要超过100ms用户就会感觉到。

卡顿经常会发生在页面转场动画上：当进行页面转场的时候，js线程要计算新页面中所有的组件，以便通知原生去展现view。如果动画是js控制的，就通常会消耗几帧。所以，组件在`componentDidMount`中进行的工作会导致转场动画的掉帧。

另一个例子是触摸事件的反馈：如果你的js线程正在忙碌着，点击TouchableOpacity会有延迟。这是因为js不能及时的处理触摸事件，导致原生view 不能及时的做出反应。
### Main thread（UI thread） frame rate【UI线程帧数】
你可能会注意到`NavigatorIOS`的性能要比`Navigator`好，这是因为`Navigator`的转场动画是由主线程控制的，js的掉帧并不能影响它。参考阅读来帮助你选择导航器http://facebook.github.io/react-native/docs/navigator-comparison.html

同样的，ScrollView的滚动永远不会受到js的影响，这是因为ScrollView完全是基于原生的主线程的。尽管会有滚动事件分发到js，但这并不影响滚动的动作。


##常见问题和解决方案
###开发模式 dev=true
在开发模式下js的性能会受到严重的影响。这是不可避免的:要提供详细的提示和错误检查js就要有大量的工作要做。

###转场动画不流畅
如上所述，



