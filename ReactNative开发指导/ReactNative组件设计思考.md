# ReactNative组件设计思考 未完

设计React组件与设计一个jquery组件或者一个原生js组件最大的区别就是状态的设计。

其余方面的设计沿用原来的设计思路就好。

##传统设计思路

按照原来设计组件的方法论，一个UI组件应该具有属性、事件、接口。

1. 属性就像一份配置文件，描述了组件应该具有的功能、外观或者初始状态。
2. 事件是组件在工作工程中，当满足某种条件的时候触发的回调函数
3. 接口是组件对象的方法，可以让组件去执行某个任务

在原来的设计理念中，并没有提出组件状态的概念，但是其也是一直存在的，通常是使用组件私有属性来存储。

比如，你设计一个按钮，那么他可能有正常状态，禁用状态，那么我们会设计一个属性disable={true|false}来通知组件初始化的具有的状态，设计2个接口disable、enable来赋予js有动态改变其状态的能力，设计2个事件onEnable、onDisable来通知回调函数组件状态发生了变化。伪代码如下：

```
//组件定义
class button{

	constructor(disable,pid){//构造函数
	
		this.disable=disable,//属性：组件初始化使用这个作为状态
		this.pid=pid;//属性：父容器的id
		
		this._disable=null,//状态:私有属性

		if(this.disable==true){//根据属性来决定初始化状态
			this.disable();
		}else{
			this.enable();
		}
		this.render();//渲染组件
	}
		
	
	enable(){
		this._disable=false;
		if(this.el)this.el.set('disable',false);
		this.fireEvent('onEnable');//触发事件
	}
	
	disable(){
		this._disable=true;
		if(this.el)this.el.set('disable',true);
		this.fireEvent('onDisable');//触发事件
	}
	
	
	render(){
		//渲染组件，状态直接影响了组件的表现和功能
		$(this.pid).innerHTML='<button disable='+this._disable+' />';
		
		//初始化对dom节点的引用
		this.el=$(this.pid).getChild();
	}
	
}

//父容器
<div id='a'></div>

//实例化组件并使用
new button(false,'a');

```

##RN设计思路

上面的示例中，表示了一个传统UI组件的设计思路，_disable就是这个button的关键状态，它直接影响了组件的表现和行为，而react架构把组件状态抽象到了一个新的高度。






