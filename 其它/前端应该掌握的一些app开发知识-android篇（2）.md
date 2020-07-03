# 前端应该掌握的一些app开发知识-android篇（2）.md

接前篇，本次主要整理一下前端应了解的android的一些重要的概念。

# ide

目前几个主流java的ide都支持android的开发，但是目前最主流的，也是官方推荐的还是AndroidStudio。所以，对前端来说，如果你想跑个android的工程来玩，还是用AndroidStudio比较好。

# android Sdk

SDK是(software development kit)软件开发工具包。被软件开发工程师用于为特定的软件包、软件框架、硬件平台、操作系统等建立应用软件的开发工具的集合。

因此，Android SDK 指的是Android专属的软件开发工具包。

# 模拟器 genymotion

对于开发者来说，模拟器是可选物品，因为你也可以选择真机调试。

如果用模拟器的话，genymotion是首选，因为它的性能是最佳的。

Genymotion是一套完整的工具，它提供了Android虚拟环境。

Genymotion支持Windows、Linux和Mac OS等操作系统，非常容易安装和使用。

# AndroidManifest.xml

学习android，不能不了解AndroidManifest.xml，他是整个安卓程序的入口和描述文件。

AndroidManifest.xml 是每个android程序中必须的文件。

它位于整个项目的根目录，描述了package中暴露的组件（activities, services, 等等），他们各自的实现类，各种能被处理的数据和启动位置。 除了能声明程序中的Activities, ContentProviders, Services, 和Intent Receivers,还能指定permissions和instrumentation。


# android四大组件

整个android应用的功能都是由四大组件来完成的，这里我们来总结下。

Activity 显示界面（显示的界面都是继承activity完成的）

Service 服务（后台运行的，可以理解为没有界面的activity）

Broadcast Receiver 广播（做广播，通知时候用到）

Content Provider  数据通信（数据之间通信，同个程序间数据，或者是不同程序间通信）

# Activity

对于前端来讲，四大组件中最应该了解的就是Activity了。

android中，每一个页面都是一个activity，它上面可以显示一些view控件也可以监听并处理用户的事件做出响应。

activity就像是页面容器，上面可以放很多的view组件。

activity本身还具有完整的生命周期事件，在页面的初始化，展现，置于后台，销毁等生命周期事件中，会触发相应的函数。类似小程序中page里的onLoad，onShow等方法。

# 前端最应该了解的view组件---webview

Android和ios都有webview。只是其引擎不同，android及ios的webview的引擎都是webkit，对Html5提供支持。

#### 如何使用

1. 添加权限：AndroidManifest.xml中必须使用许可"android.permission.INTERNET",否则会出Web page not available错误。
2. 在要Activity中生成一个WebView组件：WebView webView = new WebView(this);或者可以在activity的layout文件里添加webview控件：
3.  设置WebView基本信息：如果访问的页面中有Javascript，则webview必须设置支持Javascript。 webview.getSettings().setJavaScriptEnabled(true);  
4. 设置WevView要显示的网页：互联网用：webView.loadUrl("http://www.google.com"); 本地文件用：webView.loadUrl("file:///android_asset/XX.html");  本地文件存放在：assets文件中。
5. 如果希望点击链接由自己处理，而不是新开Android的系统browser中响应该链接。给WebView添加一个事件监听对象（WebViewClient)并重写其中的一些方法：shouldOverrideUrlLoading：对网页中超链接按钮的响应。当按下某个连接时WebViewClient会调用这个方法，并传递参数：按下的url。比如当webview内嵌网页的某个数字被点击时，它会自动认为这是一个电话请求，会传递url：tel:123,如果你不希望如此可通过重写shouldOverrideUrlLoading函数解决

#### Webview与js交互

WebView不但可以运行一段HTML代码，还有一个重要特点，就是WebView可以同Javascript互相调用。

通过addJavascriptInterface(Object obj,String interfaceName)方法将一个Java对象绑定到一个Javascript对象中，Javascript对象名就是interfaceName，作用域是Global，这样便可以扩展Javascript的API，获取Android的数据。

同时，在Java代码中也可以直接调用Javascript方法，这样就可以互相调用取得数据了，代码如下：
WebView.loadUrl("javascript:方法名()");


