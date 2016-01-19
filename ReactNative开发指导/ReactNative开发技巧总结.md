# ReactNative开发技巧总结

#样式
不支持选择器

无复用就尽量用行内，避免命名难题

不支持position=fixed，利用flexbox可以搞定。<https://medium.com/@Nayflix/building-a-soundcloud-app-with-react-native-1144b6d3773a#.n6bzcbm43>

		
RN样式没有提供类似于display=none的方式,使用BaseCss提供的hidden样式,原理是绝对定位，left=-9999
	
总结的较好：http://segmentfault.com/a/1190000002658374


#调试
在系统首页调试可以极大的提供效率，系统首页直接使用你要调试的组件作为app
	
js可以用chrome来调试

css和结构就靠刷新了，官方给的结构查看器不是很好用

在一些重要逻辑部分多用console.log，特别是在触摸事件中，有时候还可以发现隐藏问题，习惯用console.log代替一些注释


#touch系列
一般包裹在view的外面，margin写在touch上，不要写在view上，否则margin会有动画效果，padding写在view上。
	
一般使用touchOpacity，不使用touchHeighlight



#text
一定不要有padding margin，如果要有请在外面套一个view，写在view上

#listview，scrollview
必须有高，否则不能滚动，flex=1也可以，相当于给了高


