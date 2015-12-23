# ReactNative离线打包

参考地址-文档很旧：<http://facebook.github.io/react-native/docs/running-on-device-ios.html#content>

安卓打包：<https://github.com/facebook/react-native/issues/2743#issuecomment-140697340>

#打包命令说明

react-native bundle

Options:

  --entry-file        Path to the root JS file, either absolute or relative to JS root                                   [required]
  
  --platform          Either "ios" or "android"         
                                                                 
  --transformer       Specify a custom transformer to be used (absolute path)                                            [default: "/Users/babytree-mbp13/projects/xcodeProjects/AwesomeProject/node_modules/react-native/packager/transformer.js"]
  
  --dev               If false, warnings are disabled and the bundle is minified                                         [default: true]
  
  --prepack           If true, the output bundle will use the Prepack format.                                            [default: false]
  
  --bridge-config     File name of a a JSON export of __fbBatchedBridgeConfig. Used by Prepack. Ex. ./bridgeconfig.json
  
  --bundle-output     File name where to store the resulting bundle, ex. /tmp/groups.bundle                              [required]
  
  --bundle-encoding   Encoding the bundle should be written in (https://nodejs.org/api/buffer.html#buffer_buffer).       [default: "utf8"]
  
  --sourcemap-output  File name where to store the sourcemap file for resulting bundle, ex. /tmp/groups.map       
       
  --assets-dest       Directory name where to store assets referenced in the bundle                     
                 
  --verbose           Enables logging                                                                                    [default: false]


 
#ios打包步骤
1. 在工程根目录下执行打包命令，比如react-native bundle --entry-file demo/index.js --bundle-output ./ios/bundle/index.ios.jsbundle --platform ios --assets-dest ./ios/bundle --dev false，请参考上面命令说明，根据自己的情况进行修改再执行。
2. 命令执行完生成如下资源 ![2015-12-23 17.41.04](media/2015-12-23%2017.41.04.png)


2. 在xcode中添加assets【必须用Create folder references的方式，添加完是蓝色文件夹图标】和index.ios.jsbundle，如图![2015-12-23 17.35.50](media/2015-12-23%2017.35.50.png)
3. 参考官方文档，修改AppDelegate.m文件,使用OPTION 2处的代码
	```
	jsCodeLocation = [[NSBundle mainBundle] URLForResource:@"index.ios" withExtension:@"jsbundle"];
	```
4. 一切OK 运行模拟器看效果吧
 
#ios打包遇到的问题
1. 离线包如果开启了chrome调试，会访问调试服务器，而且会一直loading出不来。 
2. 如果bundle的名字是main.jsbundle,app会一直读取旧的,改名就好了。。。非常奇葩的问题，我重新删了app，clean工程都没用，就是不能用main.jsbundle这个名字。
3. 必须用Create folder references【蓝色文件夹图标】的方式引入图片的assets，否则引用不到图片

