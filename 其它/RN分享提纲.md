# RN分享提纲

# 目标

最终目标：能够在孕育app中进行rn开发。

本期目标：了解孕育中开发rn的一般过程。

# 实施项目

孕育中专家答和记录tab。

# 环境搭建

#### ios

xcode+app的ios代码+js编辑器+rn开发服务器+真机or模拟器

#### android（不推荐，建议直接真机）

jdk+androidSdk+androidStudio+app的android代码+js编辑器+rn开发服务器+真机or模拟器

#### 安装react-native cli

#### 安装xcode

#### git库下载代码


# 开发过程

1. 启动rn开发服务器
2. 启动模拟器，点开app，设置自动reload
3. 启动js代码编辑器，coding---save---看效果
4. tips:可以直接让首页显示你要开发的页面，可以快速查看效果

```
// tips用到的代码，替换index页面的最后一行
AppRegistry.registerComponent('Ask', () => require('../../../demo/demo1/animate/drag2'));
```

# 调试过程

1. console.log
2. 断点调试


# 发版过程

### 全量发布

全量发布是将rn相关资源打包进app中，是无需网络下载的。

一般过程：

1. coding结束
2. 使用全量打包命令，打出android和ios全量包
3. 将包发给android和ios的工程师即可

### 增量发布

[增量升级方案](https://github.com/cnsnake11/blog/blob/master/ReactNative%E5%BC%80%E5%8F%91%E6%8C%87%E5%AF%BC/ReactNative%E5%A2%9E%E9%87%8F%E5%8D%87%E7%BA%A7%E6%96%B9%E6%A1%88.md)

一般过程：

1. coding结束
2. 使用全量打包命令，打出android和ios全量包
3. 使用增量打包命令，打出android和ios增量包
4. 将增量包上传到后台管理系统，上线即可

### 增量包测试过程

1. 打出增量包
2. 将增量包上传到升级后台，上线标识为false
3. 手机开启测试模式（isDev=true）
4. 手动更新增量包

# 课后希望

搭建开发环境，把上面的内容都操作一下。



****************************  和下期分享的分割线  *********************************

# 开发规范

# 状态管理

# 常用组件和接口

### 导航、列表等

### 与app的交互

### 埋点

# 自定义原生组件

# 性能tips


