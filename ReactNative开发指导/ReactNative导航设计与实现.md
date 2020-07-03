# ReactNative导航设计与实现

# 前言

关于reactnaitve的导航，官方提供了2个组件，NavigatorIOS和Navigator，其中官方并不推荐使用NavigatorIOS，它不是官方维护的，不能保证及时的更新和维护。

所以本文中是以Navigator组件为基础，进行导航的设计和实现。

Navigator的劣势：Navigator组件是纯js的实现，所以在页面进行转场动画的过程中，如果js不能保证在16ms内完成其它操作的话，转场动画会有卡顿现象，后面会介绍优化的方案。

官方的Navigator组件使用方式较为灵活，本文的目的是选取一种最佳用法，并提取出通用功能应对常用场景，规范和设计项目中导航的使用。

# 定义

rn应用：全站rn应用，简称rn应用。

rn模块：部分模块使用rn，简称rn模块。

rn首页：无论是rn应用还是rn模块，进入rn页面的第一屏，简称rn首页。

nav：Navigator组件对象的简称，注意是实例化好的对象，不是类。页面间传递的导航对象统一使用此命名。

Header：自定义的导航栏组件。

# 体系结构、设计原则

一个rn应用或者一个rn模块，有且只有一个Navigator组件被定义。

在rn首页定义Navigator组件。

各个子页面统一使用首页定义的Navigator组件对象nav。

不要使用Navigator的navigationBar，请自定义导航栏组件，例如Header组件。


# Navigator组件的定义和初始化

在rn首页中的render方法中，定义一个Navigator组件，并做好以下几件事：

1. 实现好通用的renderScene方法，
2. 实现好android的物理返回按键
3. 初始化真正的rn首页

## 实现统一路由函数renderScene

renderScene函数是Navigator组件的必填函数，入参是route对象和当前的nav对象，返回值是jsx。

此函数的意思是根据传入的route，返回一个作为新页面的jsx，也就是说所有的路由算法都是在此函数中实现的。

其中route对象是一个自定义的对象，是nav.push方法中传入的对象。

此函数设计相当于门面模式，此函数是路由的统一处理器，所有的页面跳转请求都会通过此函数的计算来获得具体的jsx页面。

既然是统一的路由处理器，必然要求传入的route对象要满足统一的规则，否则无法实现统一的算法。

在此，设计route对象如下：

```
{
	name: 'page2', //名字用来做上下文判断和日志输出
	page: <Page2 />, //jsx形式的page，作为新页面的jsx
	// page: () => <Page2 />, //或者函数形式的page，此函数必须返回jsx，此jsx作为新页面的jsx
}
```
根据route对象设计，设计统一的renderScene方法如下：

```
_renderPage(route, nav) {

        if (!route.page) {
            console.error('页面导航请求没有传入page参数.');
            return null;
        }

        let page;

        if (typeof route.page === 'function') {
            page = route.page();
        } else {
            page = route.page;
        }


        let name = route.name;
        if (!name) {
            if (page) {
                name = page.type.name;
            }
        }
        console.log(`in render page ${name}`);

        return page;
    }
```

业务代码中，页面跳转的时候，只需要如下代码

```
nav.push({
	name: 'page2',
	page: <Page2 nav={nav}/>,
});
```

## android物理返回按键的处理
如果你的应用需要支持android的话，那就要实现andorid的物理返回按键的对应处理。

一般按物理返回按键要么是返回上一页面，要么是返回页面的上一状态【例如，有打开的弹窗，按返回是关闭这个弹窗】。

返回上一页面因为有通用路由器的存在，所以可以通用处理，直接使用nav.pop()即可。

但是返回页面上一状态，并不容易统一处理，所以使用基于事件扩展的方式，交给业务代码自行实现。

在此重构route对象的规则，添加事件onHardwareBackPress，如下

```
{
	name: 'page2', //名字用来做上下文判断和日志输出
	page: <Page2 />, //jsx形式的page，作为新页面的jsx
	// page: () => <Page2 />, //或者函数形式的page，此函数必须返回jsx，此jsx作为新页面的jsx
	onHardwareBackPress: () => alert('点物理按键会触发我'), // 返回false就终止统一路由器的默认动作，即终止页面返回动作,可以在此方法中实现返回页面上一状态的相关实现
}
```

android物理返回按键的统一处理代码如下，

```
componentWillMount() {

        BackAndroid.addEventListener('hardwareBackPress', () => {

            if (this.refs.nav) {

                let routes = this.refs.nav.getCurrentRoutes();
                let lastRoute = routes[routes.length - 1]; // 当前页面对应的route对象

                if (lastRoute.onHardwareBackPress) {// 先执行route注册的事件
                    let flag = lastRoute.onHardwareBackPress();
                    if (flag === false) {// 返回值为false就终止后续操作
                        return true;
                    }
                }


                if (routes.length === 1) {// 在第一页了

                    // 此处可以根据情况实现 点2次就退出应用，或者弹出rn视图等
                    
                } else {
                    
                    this.refs.nav.pop();
                    
                }
            }

            return true;
        });
    }
    
```


## 初始化真正的rn首页

此处较为简单，直接使用Navigator组件的initialRoute属性来指定初始化的route对象。

```
<Navigator initialRoute={{
           page: <Home />, // Home为伪代码，自定义的首页组件
           name: 'home',
       }} />
```

# 页面跳转

根据前面设计好的renderScene方法，直接使用如下代码,即可跳转到Page2，并将nav对象传递给了Page2.

```
nav.push({
	name: 'page2',
	page: <Page2 nav={nav}/>,
});
```

# 页面返回

页面返回直接使用

```
nav.pop();
```

# 页面转场优化

前面提到，Navigator组件完全使用js实现，由于js的单线程特点，如果在页面转场动画过程中，js干其他事情【比如渲染个某个jsx】超过了16ms，那么转场动画将不足60帧，给用户的感觉就是动画有卡顿。

为了避免这种情况，一种简单粗暴的办法就是在转场动画中不要让js来干别的事情。

那么我们如何知道转场动画什么时候结束呢，官方提供了动画交互管理器InteractionManager，示例伪代码如下：

```
InteractionManager.runAfterInteractions(() => {
      alert('哈哈 转场动画结束了！');
    });
```

大多数的场景：点击page1的某个按钮，要跳转到page2，并且page2要和服务器请求数据，根据返回的数据来渲染page2的部分or全部内容。

针对上述场景，解决方案如下，用伪代码描述：

1. page2的state至少有2个值，转场动画进行中=true，服务器查询=true
1. page2的componentWillMount方法中发起异步服务器交互请求,当请求结束setState:服务器查询=false
2. page2的componentWillMount方法中注册InteractionManager.runAfterInteractions事件，当转场结束setState:转场动画进行中=false
3. page2的render方法中，先判断（转场动画进行中=true || 服务器查询=true）就返回一个loading的提示，否则返回真正的jsx，并且此时，服务器返回的数据已经可用了

也可以参考官方文档： http://reactnative.cn/docs/0.22/performance.html#content

# 刷新的实现

目标：实现类似于html中window.reload的方法。

由于我们对route的规则限定，所以我们可以做到统一的刷新页面的逻辑。

思路是，

1. 首先获得当前页面对应的route对象
2. 然后获取route中的page属性，page属性可能是当前页面的jsx，也可能是可以产生当前页面jsx的方法
3. 最后使用官方Navigator组件提供的replace方法，来用新的route替换掉原有的route

示例参考代码如下：

```
/**
     * 刷新页面，route可以为空，会刷新当前页面
     * @param nav
     * @param route
     */
   refresh(nav, route) {

        if (!route) {
            let routes = nav.getCurrentRoutes();
            let length = routes.length;
            route = routes[length - 1]; // 使用当前页对应的route
        }

        // todo 最好的方式是直接使用route.page，但是不好使，这种写法只支持一层节点，如果有多层会有问题
        // todo 暂时未处理page是function的情况
        let Tag = route.page.type;
        nav.replace({
            page: <Tag {...route.page.props} />,
        });

    }
```

然后业务代码中这样调用,当前页面就被刷新了。

```
Util.refresh(nav); //Util是伪代码，是你定义refresh方法的对应对象

```


# rn首页直接跳转子页面

如果你开发的是rn模块【rn模块嵌入到已有app中，定义可以参考前面定义一节】，可能进入rn模块的入口会很多，比如，用rn开发一个论坛模块，正常入口进来是直接展现帖子列表，也可能会有点击某个其它按钮【此按钮是不是rn的】会直接跳转到某个帖子的详情页。

使用官方Navigator组件提供的initialRouteStack属性，可以完美的解决此问题，官方文档对此属性的说明如下：提供一个路由集合用来初始化。如果没有设置初始路由的话则必须设置该属性。如果没有提供该属性，它将被默认设置成一个只含有initialRoute的数组。

说白了就是，initialRouteStack要定义一个数组，里面是很多route对象，然后Navigator对象会展现到最后一个，而且数组中的其他route也都被初始化过了，你想返回到任何一个route都是可以的，是不是爽歪歪了。

给个示例代码吧，这是我项目中真正的代码，请当伪代码来阅读:

```
getInitialRouteStack() {

        let props = this.getProps();

        let detailId = props.detailId;
        if (detailId) { // 如果传入了详情id，那么跳转到详情页
            return [{name: 'home', },
            {
                page: <AskDetail data={{id: detailId, }}/>,
                backIsClose: true,
            }];
        }


        let wantAsk = props.wantAsk;
        if (wantAsk === true || wantAsk === 'true') { // 如果传入了提问属性=true，那么直接跳转到提问页面
            return [{name: 'home', },
            {
                page: <WantAsk backIsClose={true}/>,
                backIsClose: true,
            }];
        }

        // 跳转到首页
        return [{name: 'home', }];

    }
```

# 实现代码参考：
根据以上设计思路，笔者封装了一个Navigator组件，是对官方的navigator组件进行了一层封装，供大家参考：

```
import React from "react-native";

const {
    Platform,
    Animated,
    View,
    DeviceEventEmitter,
    Dimensions,
    Navigator,
    BackAndroid,
    } = React;

class Navigator2 extends React.Component {

    componentWillMount() {

        BackAndroid.addEventListener('hardwareBackPress', () => {

            if (this.refs.nav) {

                let routes = this.refs.nav.getCurrentRoutes();
                let lastRoute = routes[routes.length - 1];

                if (lastRoute.onHardwareBackPress) {// 先执行route注册的事件
                    let flag = lastRoute.onHardwareBackPress();
                    if (flag === false) {// 返回值为false就终止后续操作
                        return true;
                    }
                }


                if (routes.length === 1) {// 在第一页了

                    if (this.props.nav) {// 父页面仍有nav
                        this.props.nav.pop();
                    }

                    if (this.props.onHardwareBackPressInFirstPage) {
                        this.props.onHardwareBackPressInFirstPage();
                    }

                } else {

                    if (lastRoute.backIsClose === true) {
                        if (this.props.onHardwareBackPressInFirstPage) {
                            this.props.onHardwareBackPressInFirstPage();
                        }
                    } else {
                        this.refs.nav.pop();
                    }
                }
            }

            return true;
        });
    }


    getLastRoute() {
        if (this.refs.nav) {
            let routes = this.getCurrentRoutes();
            let lastRoute = routes[routes.length - 1];
            return lastRoute;
        }

        return null;
    }

    render() {
        return <Navigator renderScene={this._renderPage.bind(this)}
                          {...this.props}
                          ref='nav'
            />;
    }


    _renderPage(route, nav) {

        if (!route.page) {
            console.error('页面导航请求没有传入page参数.');
            return null;
        }

        let page;

        if (typeof route.page === 'function') {
            page = route.page();
        } else {
            page = route.page;
        }


        let name = route.name;
        if (!name) {
            if (page) {
                name = page.type.name;
            }
        }
        console.log(`in render page ${name}`);

        return page;
    }

    // todo 以下的方法为实现原版navigator的方法，这样做不好，但是没想到其它好办法
    getCurrentRoutes() {
        return this.refs.nav.getCurrentRoutes(...arguments);
    }
    jumpBack() {
        return this.refs.nav.jumpBack(...arguments);
    }
    jumpForward() {
        return this.refs.nav.jumpForward(...arguments);
    }
    jumpTo(route) {
        return this.refs.nav.jumpTo(...arguments);
    }
    push(route) {
        return this.refs.nav.push(...arguments);
    }
    pop() {
        return this.refs.nav.pop(...arguments);
    }
    replace(route) {
        return this.refs.nav.replace(...arguments);
    }
    replaceAtIndex(route, index) {
        return this.refs.nav.replaceAtIndex(...arguments);
    }
    replacePrevious(route) {
        return this.refs.nav.replacePrevious(...arguments);
    }
    immediatelyResetRouteStack(routeStack) {
        return this.refs.nav.immediatelyResetRouteStack(...arguments);
    }
    popToRoute(route) {
        return this.refs.nav.popToRoute(...arguments);
    }
    popToTop() {
        return this.refs.nav.popToTop(...arguments);
    }

}

module.exports = Navigator2;

```

#参考地址

http://reactnative.cn/docs/0.21/navigator.html#content

http://reactnative.cn/docs/0.22/performance.html#content


