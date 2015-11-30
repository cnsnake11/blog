# react-native的性能

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
如上所述，`Navigator`的转场动画是js控制的。就比如说从右推进来的这个动画效果：每一帧新页面都会从右像左移动一点，其实就是改变新页面的x轴坐标。在这过程中的每一帧，js都要告诉主线程新的x坐标是多少。如果js被别的任务锁定了，那这一帧就不能及时的更新x坐标了，你就会觉得动画卡顿了。

这个问题的最有效的解决办法就是把js控制的动画效果让主线程自己来控制。如果套用上面的场景可以这样重构，在动画的开始，js就计算好这次动画的所有x坐标值的数组，传递给主线程。这样的话，动画开始后，就不受js性能的影响了，就算动画刚开始有一些卡顿，你可能也注意不到。

但是，目前`Navigator`还不是这样实现的，所以，我们提供了`InteractionManager`来帮助你在转场动画的过程中，新页面只渲染必要的少量的内容。

`InteractionManager.runAfterInteractions`只有一个函数类型的参数，当转场动画结束，这个回调函数就会被触发(所有基于`Animated`API的动画都会触发InteractionManager.runAfterInteractions)。

示例代码如下：

```
class ExpensiveScene extends React.Component {
  constructor(props, context) {
    super(props, context);
    this.state = {renderPlaceholderOnly: true};
  }

  componentDidMount() {
    InteractionManager.runAfterInteractions(() => {
      this.setState({renderPlaceholderOnly: false});
    });
  }

  render() {
    if (this.state.renderPlaceholderOnly) {
      return this._renderPlaceholderView();
    }

    return (
      <View>
        <Text>Your full view goes here</Text>
      </View>
    );
  }


  _renderPlaceholderView() {
    return (
      <View>
        <Text>Loading...</Text>
      </View>
    );
  }
};
```
你不仅可以渲染一个loading指示器，也可以渲染你新页面内容的一小部分。比如说，当你打开Facebook你会看到将要放入文本的灰色的矩形。如果你要在新页面要放入一个地图，你可以放一个灰色的占位符或者一个loading直到动画结束，否则地图的初始化一定会导致转场动画的掉帧。

### 大数据量的ListView初始化慢或者滚动卡顿
我们提供了一些方法来调优，但是尚没有万能的办法，你可以尝试以下选项。
####initialListSize 初始帧渲染行数
这个属性定义了第一帧渲染list的行数。如果我们希望某些内容非常快的展现在页面上，可以设置`initialListSize`等于1，其余的行会在后边的帧里来渲染。每一帧渲染的行数是由`pageSize`决定的。
####pageSize 每一帧渲染行数
初始化使用`initialListSize`，`pageSzie`决定了每一帧渲染的行数。默认值是1，如果你的view很小而且渲染很简单，你可以加大这个数量。你可以通过测试来找到你满足你需求的值。
####scrollRenderAheadDistance 触发渲染的距离
在距离显示区域多远就开始渲染。

如果你的列表有2000行，那么一次渲染所有会耗费大量内存和计算资源。`scrollRenderAheadDistance`可以让你定义距离显示区域多远之内的rows可以被渲染。

####removeClippedSubviews删除屏幕外子视图
此值为true表示屏幕外的字视图(行容器是overflow=hidden的)会被从原生视图中移除。他可以提升列表滚动的性能。这个属性默认值就是ture。
这个属性对大数据列表非常重要。在安卓端overflow的默认值就是hidden，但是在ios端需要手动设置overflow的值为hidden在行容器上。

###不需要立即展现的组件渲染很慢
这通常是由listvew引起的，你先想办法调整一下listview。上面我们也提供了很多手段来分步渲染视图。Listview也可以横向展现，好好利用这一点。

###重新渲染那些很少改变的视图很慢
Listview可以使用`rowHasChanged`方法来降低diff判断的计算损耗。如果你使用了不可变数据类型，使用`=`就可以简单的判断是否是同一对象了。

同样的，`shouldComponentUpdate`可以精确的描述出需要重新渲染的状态。如果你正在写纯组件（render方法的返回完全由props和state来决定），使用`PureRenderMixin`可以帮助你提升性能。同样的，不可变数据类型也同样会提升性能和降低代码复杂度，特别是在大数据量的对象深比对上。

###js线程在同一时间做太多的事就会掉帧
过场动画卡顿就是因为这个原因，在某些场景下也会出现这个问题。使用InteractionManager是比较好的办法，但当你由于某些原因不能延迟执行任务时，你可以试试LayoutAnimatio你。

AnimatedApi的动画效果依赖于js，而LaoutAnimation使用了CoreAnimation 完全不受js线程和主线程掉帧的影响。

你可以查看相关文档来学习如何使用。

注意，只支持iOS。而且，只支持静态动画。如果需要动画过程有交互，还要使用Animated。

















