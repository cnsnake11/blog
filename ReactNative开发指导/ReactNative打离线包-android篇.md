# ReactNative打离线包-android篇


官方文档：<http://facebook.github.io/react-native/docs/running-on-device-android.html#content>

官方文档2：<http://facebook.github.io/react-native/docs/signed-apk-android.html#content>

离线包就是把RN和你写的js图片等资源都打包放入app，不需要走网络下载。

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


#安卓打包步骤

1. 在工程根目录下执行打包命令，比如``` react-native bundle --entry-file demo/index.js --bundle-output ./android/app/src/main/assets/index.android.jsbundle --platform android --assets-dest ./android/app/src/main/res/ --dev false ```请参考上面命令说明，根据自己的情况进行修改再执行。注意要先保证[./android/app/src/main/assets/]文件夹存在。
1. 命令执行完生成资源如图![2015-12-24 11.05.31](media/2015-12-24%2011.05.31.png)
1. 保证MainActivity.java中的setBundleAssetName与你的jsbundle文件名一致，比如`.setBundleAssetName("index.android.jsbundle")`就与我生成的资源名一致
2. 一切OK 打包测试吧




