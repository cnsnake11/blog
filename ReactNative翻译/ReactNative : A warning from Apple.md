# ReactNative : A warning from Apple

2017-03-08这一天，突然听到ios的同事说，收到了苹果的警告email，说rn有可能触犯了苹果的审核规则，有可能被拒。所以，去rn的官方看，发现已经有对应的issue被提出，rn官方也在积极的回应这件事，此处翻译一下issue的关键内容。

需要看结论的同学直接看最后一段即可。

原文地址：https://github.com/facebook/react-native/issues/12778

# 问题

I received a warning from Apple this morning , how to solve it :

今天早上我收到了苹果的警告邮件，该如何处理呢：

Dear Developer,

Your app, extension, and/or linked framework appears to contain code designed explicitly with the capability to change your app’s behavior or functionality after App Review approval, which is not in compliance with section 3.3.2 of the Apple Developer Program License Agreement and App Store Review Guideline 2.5.2. This code, combined with a remote resource, can facilitate significant changes to your app’s behavior compared to when it was initially reviewed for the App Store. While you may not be using this functionality currently, it has the potential to load private frameworks, private methods, and enable future feature changes.

您的app或者您app中使用的框架，被发现含有在苹果审核之后能修改app功能和行为的代码，这不符合Apple Developer Program License Agreement中的3.3.2章节，也不符合App Store Review Guideline的2.5.2章节。这些代码可以对app进行完全的修改，使其和审核时候的初始版本有很大的差异。也许你现在并没有使用这个功能，但是仍具有调用私有框架，私有方法，改变产品特性的潜在可能。

This includes any code which passes arbitrary parameters to dynamic methods such as dlopen(), dlsym(), respondsToSelector:, performSelector:, method_exchangeImplementations(), and running remote scripts in order to change app behavior or call SPI, based on the contents of the downloaded script. Even if the remote resource is not intentionally malicious, it could easily be hijacked via a Man In The Middle (MiTM) attack, which can pose a serious security vulnerability to users of your app.

像这种动态调用函数的代码，比如dlopen(), dlsym(), respondsToSelector:, performSelector:, method_exchangeImplementations(),还有为了改变app行为的执行远程脚本的行为，也叫SPI。尽管远程资源并不是有害的，但却很容易通过中间人攻击被劫持，这是一个很严重的安全漏洞。

Please perform an in-depth review of your app and remove any code, frameworks, or SDKs that fall in line with the functionality described above before submitting the next update for your app for review.

请仔细检查你的代码，并删除符合上述描述的代码，然后重新提交审核。

Best regards,

# grabbou解释了相关的苹果审核规则

3.3.2 of Apple Developer Program License:

苹果开发者协议的3.3.2章节：

An Application may not download or install executable code. Interpreted code may only be used in an Application if all scripts, code and interpreters are packaged in the Application and not downloaded. The only exception to the foregoing is scripts and code downloaded and run by Apple's built-in WebKit framework or JavascriptCore, provided that such scripts and code do not change the primary purpose of the Application by providing features or functionality that are inconsistent with the intended and advertised purpose of the Application as submitted to the App Store.

app不允许下载安装可执行代码。解释型的代码只允许打包到app中才可以被执行，下载的解释型代码不允许被执行。有一种情况是例外的，通过WebKit或者JavascriptCore执行的远程代码，并且这些远程代码并没有违反app提交审核时候的描述和声明。

explicitly mentions that: An Application may not download or install executable code

规则中明确的指出了：app不能下载安装可以执行代码。

React Native does none of these. And so, using React Native doesn't expose you to the aforementioned issue.

React Native并没有违反这一条。所以，使用React Native也并没有违反这一条。

As @ide mentioned, there are reports this message is addressed to those using Rollout.io. My assumption is that it uses aforementioned methods to patch executable code.

就像@ide提到的，使用Rollout的都被发警告邮件了。我的猜测是，Rollout使用了上述的方法来给可执行代码打补丁了。

With React Native, you can do so called OTA update, but this is updating Javascript, not native code. Whenever native code changes, you have to make a new release. That OTA update of Javascript code is explicitly allowed in the 3.3.2:The only exception to the foregoing is scripts and code downloaded and run by Apple's built-in WebKit framework or JavascriptCore。

使用React Native可以进行OTA在线更新，但是这是更新Javascript的代码，并不是更新原生代码。当有原生代码改变，你必须要重新发布版本提交审核。在开发者协议的3.3.2章节中，OTA更新Javascript代码是明确指出被允许的：有一种情况是例外的，通过WebKit或者JavascriptCore执行的远程代码，并且这些远程代码并没有违反app提交审核时候的描述和声明。

I believe everyone should follow up on this issue with Apple and sort out what's exactly causing the warning.

我建议大家还是仔细检查一下代码或者和apple沟通，看究竟是什么问题触发的警告。

# 官方最后给的结论

From reading the message from Apple and the data points we've observed, this warning is not about React Native.

根据苹果的信息和我们目前获得的情况，这个警告和React Native无关。

### On the technical concern of dynamically executing Objective-C described in Apple's email:苹果邮件中关于动态执行oc代码的分析

Apple's message reads to me that they're concerned about libraries like Rollout and JSPatch, which expose uncontrolled and direct access to native APIs (including private APIs) or enable dynamic loading of native code. Rollout and JSPatch are the only two libraries I've heard to be correlated with the warning.

苹果的描述在我看来，他们关系的是Rollout和JSPatch这类的库，这类的库可以在运行时直接进行无控制的并且直接的对原生代码的调用，甚至包括私有api。Rollout和JSPatch是我唯一知道和这个警告有关的库。

React Native is different from those libraries because it doesn't expose uncontrolled access to native APIs at runtime. Instead, the developer writes native modules that define some functions the app can call from JavaScript, like setting a timer or playing a sound. This is the same strategy that "hybrid" apps that use a UIWebView/WKWebView have been using for many years. From a technical perspective, React Native is basically a hybrid app except that it calls into more UI APIs.

而React Native和他们是不同的，RN在运行时候并没有暴露原生API的能力。相反，开发者必须按照RN的规范进行原生方法的定义，这样js才能调用得到，比如设置一个定时器，或者播放一段音频等。这种策略，hybrid应用已经使用了很长时间了。从技术的角度上说，React Native也是一个hybrid应用，只不过RN使用的是原生UI。

Technically it is possible for a WebView app or a React Native app also to contain code that exposes uncontrolled access to native APIs. This could happen unintentionally; someone using React Native might also use Rollout. But this isn't something specific to or systemic about React Native nor WebViews anyway

其实无论是WebView应用还是RN应用都可以做到运行时动态执行任何原生API。但这不是有意识的设计；比如你用RN也可以同时使用Rollout。但这不是RN或者Webview设计的初衷。

### On the strategy of calling native code from JavaScript:js调用原生代码的分析

Apple has been fine with WebViews in apps since the beginning of the App Store, including WebViews that make calls to native code, like in the Quip app. WKWebView even has APIs for native and web code to communicate. It's OK for a WebView to call out to your native code that saves data to disk, registers for push notifications, plays a sound, and so on.

在最开始，Apple就允许app中含有WebViews，并且允许WebViews中的js可以调用原生代码，就像Quip应用。WKWebView还发布了原生和web通信的接口。webview中的js可以通过调用原生api进行保存数据到存储，注册接收通知，播放音频，等等。

React Native is very much like a WebView except it calls out to one more native API (UIKit) for views and animations instead of using HTML.

React Native的机制其实和webview非常类似，只不过RN使用了更多的原生api来渲染页面和动画，而不是用html。

What neither WebViews nor React Native do is expose the ability to dynamically call any native method at runtime. You write regular Objective-C methods that are statically analyzed by Xcode and Apple and it's no easier to call unauthorized, private APIs.

webview和RN都没有暴露运行时动态执行原生代码的功能。你必须仍按照xcode和apple的规范编写oc的方法，并且使用静态编译的方式来执行，很难执行那些未授权的或者私有的api。

### Data points

The developers who received this email were using Rollout or JSPatch in their apps. People who are using only React Native or libraries/frameworks like CodePush and Expo are not affected and are continuing to have their apps accepted by the App Store review.

收到苹果警告的开发者一般是使用了Rollout或者jsPatch。如果仅仅是使用了RN，或者codepush等，并不会受到影响。

### Recommendations 建议

If you're writing a React Native or WebView app, be sure not to expose dynamic, uncontrolled access to native APIs and you should be OK. There was one third-party RN module that did this and the maintainers have taken it down. And in accordance with the iOS developer terms, make sure your updates don't change the primary purpose of your app and that the changes are consistent with the intended and advertised purpose of your app.

如果你正在进行RN或者webview应用的开发，请确保不要使用动态执行原生代码的功能，这样就不会有问题。有一个第三方的RN模块使用了类似的功能，他的开发者已经将其去掉了。并且，就像ios开发团队那样，保证你的在线升级不要违反app在提交审核时候声明的原则。

