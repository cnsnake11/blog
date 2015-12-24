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



