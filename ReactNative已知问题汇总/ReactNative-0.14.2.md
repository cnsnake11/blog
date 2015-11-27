# ReactNative-0.14.2

[toc]

##如果引入一个模块，但是这个模块没有写【module.exports】，报的错误描述不对
![](media/14486188240052.jpg)
##定义组件不能使用class name extends Component的形式，有state取不到的bug，改成React.createClass后就好了

##TextInput的borederColor,color在安卓下不好使用

##如果listview有父节点但不满足以下条件，就会出现onEndReached不停触发的bug

####所有的祖宗节点都必须定义flex=1。
其实是因为没定义flex，高度就是无限高，就不停触发了，所以只要给固定高就行了

####所有的祖宗节点不能有ScrollVIew，只能是View。
解决方案：用listview的renderHeader和renderFooter方法来创建头尾，而不要嵌套用scrollview

感觉耦合略强.本不是列表组件的部分，也得和列表产生依赖。

##ListView的Android版本不支持sticky-header特性
https://github.com/facebook/react-native/issues/2700

##ListView的数据源切换导致sectionHeader重新渲染状态重置的bug
如果切换的数据源的dataSource.cloneWithRows传入的数组中没有元素，会导致已有的section都重新渲染。section相关的状态都会重置，相当于重新创建。【在RN0.15.0下，状态不会重置了，但是还会重新渲染，sectionHeader部分会空白一阵】

解决方案：可以让第二个listview加载任何一个数据就可以了，模拟的空数据也可以，比如这样的[{}].
##android不支持textDecoration系列

##安卓图片contain不管用，显示不对
https://github.com/facebook/react-native/issues/4031

##	安卓边框圆角单独设置不管用
https://github.com/facebook/react-native/issues/3040

设置全部borderRadius是管用的

##text不支持样式text-overflow
可以用属性 numberOfLines 替代

##	listview如果来回切换数据源，有时候会导致onEndReached事件不能触发
在RN0.15.0版本下，没有再复现过，当做已经修复了吧。


