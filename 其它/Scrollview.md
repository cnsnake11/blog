
一个包装了平台的ScrollView（滚动视图）的组件，同时还集成了触摸锁定的“响应者”系统。

ScrollView和ListView/FlatList应该如何选择？ScrollView会简单粗暴地把所有子元素一次性全部渲染出来。其原理浅显易懂，使用上自然也最简单。然而这样简单的渲染逻辑自然带来了性能上的不足。想象一下你有一个特别长的列表需要显示，可能有好几屏的高度。创建和渲染那些屏幕以外的JS组件和原生视图，显然对于渲染性能和内存占用都是一种极大的拖累和浪费。

这就是为什么我们还有专门的ListView组件。ListView会惰性渲染子元素，只在它们将要出现在屏幕中时开始渲染。这种惰性渲染逻辑要复杂很多，因而API在使用上也更为繁琐。除非你要渲染的数据特别少，否则你都应该尽量使用ListView，哪怕它们用起来更麻烦。

FlatList是0.43版本开始新出的改进版的ListView，性能更优，但可能不够稳定，尚待时间考验。


# 常用属性

#### contentContainerStyle

这些样式会应用到一个内层的内容容器上，所有的子视图都会包裹在内容容器内

#### onMomentumScrollStart 

滚动动画开始时调用此函数。

#### onMomentumScrollEnd 

滚动动画结束时调用此函数。

#### onScroll

在滚动的过程中，每帧最多调用一次此回调函数。调用的频率可以用scrollEventThrottle属性来控制。

#### refreshControl 

指定RefreshControl组件，用于为ScrollView提供下拉刷新功能。

#### scrollEventThrottle

这个属性控制在滚动过程中，scroll事件被调用的频率（单位是每秒事件数量）。更大的数值能够更及时的跟踪滚动位置，不过可能会带来性能问题，因为更多的信息会通过bridge传递。默认值为0，意味着每次视图被滚动，scroll事件只会被调用一次。


# 常用方法

#### scrollTo(y: number | { x?: number, y?: number, animated?: boolean }, x: number, animated: boolean) 

滚动到指定的x, y偏移处。第三个参数为是否启用平滑滚动动画。

使用示例:

scrollTo({x: 0, y: 0, animated: true})

#### scrollToEnd(options?) 

滚动到视图底部（水平方向的视图则滚动到最右边）。

加上动画参数 scrollToEnd({animated: true})则启用平滑滚动动画，或是调用 scrollToEnd({animated: false})来立即跳转。如果不使用参数，则animated选项默认启用。
