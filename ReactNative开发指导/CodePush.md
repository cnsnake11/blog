#CodePush

https://github.com/Microsoft/react-native-code-push

#重要说明

<http://microsoft.github.io/code-push/faq/index.html#3-how-should-i-interpret-the-beta-status-of-the-service>

While the service is in beta, we don’t recommend using it for doing production deployments, since we don’t currently offer any specific SLA.因为现在是beta版本，不建议用做生产版本的发布。

确实不能用于生产,目前只在美国有服务器，大陆访问非常慢。<https://github.com/Microsoft/code-push/issues/70>

所以，增量升级只能自力更生了。

#工作方式
可以只升级Rn的js和图片，不用使用二进制包升级。

通过CodePush服务器进行升级。

**todo能否用自己的服务器？**

原生代码的改动是不能通过codepush升级的。

# 支持平台

Android (asset support coming soon!)

安卓暂不支持asset

#基本流程
1. 安装CodePush CLI
2. 创建codePush账户
3. 注册app到codepush服务器
4. app中加入codepush客户端代码，并开发升级相关代码。 
5. 发布更新到codepush服务器
6. app收到了升级推送


具体配置过程请参考官网<http://microsoft.github.io/code-push/>

网上资料参考<http://blog.csdn.net/oiken/article/details/50279871>


#下一步工作

因为codepush服务器端不开源，而且服务器还在国外的关系，不能保证服务端的稳定性，所以很遗憾不能应用于真正的项目中。

针对目前的情况，制定下一步工作如下：

1. 研读codepush客户端源码，理解android和ios在客户端下载资源文件后，如何切换到新资源的方法。
2. 设计服务端，服务端仅提供文件资源下载服务和升级信息描述服务即可，不需要较复杂的逻辑。
3. 设计自动化打包解决方案，包括升级补丁打包，完整app打包。





