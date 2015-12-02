# ReactNative-0.15.0

[toc]


##当图片位置发生调整，还加载原来位置
必须关了调试服务器，重新启动调试服务器，否则还加载原来的位置，并报错。

场景是把一个图剪切到新的位置。


## 安卓，textinput，当高度较小的时候文字显示不全
https://github.com/facebook/react-native/issues/3182

https://github.com/facebook/react-native/pull/2730

解决方案：设置padding=0，同时保证高度大于等于20就正常了


##Textinput不支持直接获得value
http://stackoverflow.com/questions/32913338/react-native-get-textinput-value

查遍了资料，翻看了源码，确实没有直接获得value的办法，放弃了。

官方给的方案是通过onchange不停的记录value，真不知道为啥要这样设计，太反人类了。

##安卓textinput不支持圆角
理论上可以利用外层view来进行圆角工作，未测试

##在【render中】获得不到同级组件的ref,因为组件还没渲染完，所以没有生成refs.
http://stackoverflow.com/questions/32680725/show-drawerlayoutandroid-via-toolbarandroid-oniconclicked

首先这不是一个bug。

这个问题的实质是，因为在render中，就想依赖同级组件，但是同级组件不能保证已经渲染完成，所以RN就干脆让你都取不到。

如果不是在render中，就没有这个问题了

this.refs['DRAWER_REF'] will only be accessible from the component that renders the drawer. If you want to access it from some component down the view hierarchy you need to either: pass ref down the view hierarchy as a property OR pass a callback method down the view hierarchy that will be defined on drawer parent component level


####解决方案1。
最后不得不把所有状态放到父组件中，然后通过props传递给子组件，达到了同级组件之间通信。

这样会导致设计组件状态时候要考虑组件间通信的问题，有时候子组件状态不得不升级到父组件中，影响设计逻辑

####解决方案2 
让子组件初始化的时候把this传递给父组件，这样父组件就有所有子组件的引用了，所有子组件通过父组件来来获得同级组件对象

这样做，如果组件渲染顺序不是预期的，仍然会拿不到。

####此问题还需要思考更好的解决方案

##经过各种测试，终于找到了一个隐藏组件的好办法
todo



##text组件的opacity从他的父亲view继承，且不能修改
https://github.com/facebook/react-native/issues/1314

解决方案：父组件使用rgba来写透明度，而不是opacity。请参考：https://github.com/facebook/react-native/commit/3c04bfcf53ced10aa3afc378b2b35afbfebc9678

##绝对定位是基于父组件的，这与css很不同。
要想基于页面进行定位，必须要放到根节点下面，不知道还有没有别的解决方案。参考地址：https://github.com/facebook/react-native/issues/3210

这是一个解决方案，是用原生来做的，只支持ios。地址：https://github.com/brentvatne/react-native-overlay

## 不支持zIndex
参考地址：https://github.com/facebook/react-native/issues/1076

##安卓和ios绝对定位对overflow的反应不同
```
<View style={css.parent}>
	<View style={css.child}>
	</View>
</View>
        
parent:{
   width:200,
   height:200,
   backgroundColor:'green',
   overflow:'visible',
},

child:{
   position:'absolute',
   top: 10,
   bottom: 0,
   left: 10,
   right: 0,

   width:400,
   height:500,
   backgroundColor:'red',

},             
```
1. parent上overflow=hidden，安卓ios表现一致，parent会挡住绝对定位的child
2. parent上overflow=visible，安卓parent仍然会挡住child，ios不会挡住。

注意：overflow在安卓下默认是hidden，在ios下默认是visible

##overflow在安卓下默认是hidden，在ios下默认是visible

overflow在安卓下默认是hidden，在ios下默认是visible
##TouchableOpacity和TouchableHighlight，第一次点击不触发press

场景：
 
1. 当touch组件在listview or scrollView的时候
2. 焦点在某个输入框中

解决方案：ListView or ScrollView上加入属性keyboardShouldPersistTaps={true}

 
####scrollview的keyboardShouldPersistTaps bool 
When false, tapping outside of the focused text input when the keyboard is up dismisses the keyboard. When true, the scroll view will not catch taps, and the keyboard will not dismiss automatically. The default value is false.

false的时候scrollview会catch事件，然后消失键盘，事件不会传递到touch。

true的时候相反。

经过测试这个属性ListView组件也可以用。

参考地址：https://github.com/facebook/react-native/issues/4087


##安卓不支持textDecorationLine的所有值
参考地址：https://github.com/facebook/react-native/pull/3816

##遇到了一个require有循环引用的情况，然后会报错

报错如下：

Invariant Violation: Element type is invalid: expected a string (for built-in components) or a class/function (for composite components) but got: object. Check the render method of `Result`.

解决方案：当时是header依赖search，search依赖result，result依赖header。然后我把header对search的依赖去掉，移动到更上一级中，就好了。

这个问题未来可能是个大坑。












