高性能的简单列表组件，支持下面这些常用的功能：

完全跨平台。
支持水平布局模式。
行组件显示或隐藏时可配置回调事件。
支持单独的头部组件。
支持单独的尾部组件。
支持自定义行间分隔线。
支持下拉刷新。
支持上拉加载。
支持跳转到指定行（ScrollToIndex）。


本组件实质是基于<VirtualizedList>组件的封装，因此也有下面这些需要注意的事项：

1. 当某行滑出渲染区域之外后，其内部状态将不会保留。请确保你在行组件以外的地方保留了数据。
1. 为了优化内存占用同时保持滑动的流畅，列表内容会在屏幕外异步绘制。这意味着如果用户滑动的速度超过渲染的速度，则会先看到空白的内容。这是为了优化不得不作出的妥协，而我们也在设法持续改进。
1. 本组件继承自PureComponent而非通常的Component，这意味着如果其props在浅比较中是相等的，则不会重新渲染。所以请先检查你的renderItem函数所依赖的props数据（包括data属性以及可能用到的父组件的state），如果是一个引用类型（Object或者数组都是引用类型），则需要先修改其引用地址（比如先复制到一个新的Object或者数组中），然后再修改其值，否则界面很可能不会刷新。（译注：这一段不了解的朋友建议先学习下js中的基本类型和引用类型。）
1. 默认情况下每行都需要提供一个不重复的key属性。你也可以提供一个keyExtractor函数来生成key。


# 常用属性

#### data: ?Array<ItemT> 

为了简化起见，data属性目前只支持普通数组。如果需要使用其他特殊数据结构，例如immutable数组，请直接使用更底层的VirtualizedList组件。


#### getItemLayout?: (data: ?Array<ItemT>, index: number) =>
  {length: number, offset: number, index: number} 

getItemLayout是一个可选的优化，用于避免动态测量内容尺寸的开销，不过前提是你可以提前知道内容的高度。如果你的行高是固定的，getItemLayout用起来就既高效又简单，类似下面这样：

getItemLayout={(data, index) => ( {length: 行高, offset: 行高 * index, index} )}
注意如果你指定了SeparatorComponent，请把分隔线的尺寸也考虑到offset的计算之中。


#### initialNumToRender: number 

指定一开始渲染的元素数量，最好刚刚够填满一个屏幕，这样保证了用最短的时间给用户呈现可见的内容。注意这第一批次渲染的元素不会在滑动过程中被卸载，这样是为了保证用户执行返回顶部的操作时，不需要重新渲染首批元素。


#### initialScrollIndex?: ?number 

开始时屏幕顶端的元素是列表中的第 initialScrollIndex 个元素, 而不是第一个元素。设置这个属性会关闭对“滚动到顶端”这个动作的优化（参见VirtualizedList 的 initialNumToRender 属性)。位于 initialScrollIndex 位置的元素总是会被立刻渲染。需要先设置 getItemLayout 属性。

#### keyExtractor: (item: ItemT, index: number) => string 

此函数用于为给定的item生成一个不重复的key。Key的作用是使React能够区分同类元素的不同个体，以便在刷新时能够确定其变化的位置，减少重新渲染的开销。若不指定此函数，则默认抽取item.key作为key值。若item.key也不存在，则使用数组下标。


#### onEndReached?: ?(info: {distanceFromEnd: number}) => void 

当列表被滚动到距离内容最底部不足onEndReachedThreshold的距离时调用。

#### onEndReachedThreshold?: ?number 

决定当距离内容最底部还有多远时触发onEndReached回调。注意此参数是一个比值而非像素单位。比如，0.5表示距离内容最底部的距离为当前列表可见长度的一半时触发。

# 常用方法

#### scrollToEnd(params?: object) 

滚动到底部。如果不设置getItemLayout属性的话，可能会比较卡。

#### scrollToIndex(params: object) 

将位于指定位置的元素滚动到可视区的指定位置，当 viewPosition 为 0 时将它滚动到屏幕顶部，为 1 时将它滚动到屏幕底部，为 0.5 时将它滚动到屏幕中央。

如果不设置getItemLayout属性的话，无法跳转到当前可视区域以外的位置。

#### scrollToItem(params: object) 

这个方法会顺序遍历元素。尽可能使用 scrollToIndex 。 如果不设置getItemLayout属性的话，可能会比较卡。

#### scrollToOffset(params: object) 

滚动列表到指定的偏移（以像素为单位），等同于 ScrollView 的 scrollTo 方法。
