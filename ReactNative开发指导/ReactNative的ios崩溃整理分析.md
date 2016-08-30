# ReactNative的ios崩溃整理分析

共12种异常,如下：

```
wanghonglu  18:14
>---------------------- Row   1 -----------------------<
=> Start Unable to execute JS call: __fbBatchedBridge is undefined 
	-> translating『 0x100ac2b60x100aa8004 』=> RCTFatal /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTAssert.m: line 120
	-> translating『 0x100ac2b60 』=> -[RCTBatchedBridge stopLoadingWithError:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 476
	-> translating『 0x1006d72fc 』=> main /Users/wanghonglu/Desktop/project/pregnancy-v2-ios/pregnancy/pregnancy/main.m: line 13
=> End Unable to execute JS call: __fbBatchedBridge is undefined 
>------------------------------------------------------<


>---------------------- Row   2 -----------------------<
=> Start Application received signal SIGSEGV 
	-> translating『 0x1012891bc 』=> 
	-> translating『 0x100abf734 』=> __64-[RCTJSCExecutor executeApplicationScript:sourceURL:onComplete:]_block_invoke /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 515
	-> translating『 0x100abf944 』=> -[RCTJSCExecutor executeBlockOnJavaScriptQueue:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 547
	-> translating『 0x100abdc3c 』=> +[RCTJSCExecutor runRunLoopThread] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 151
=> End Application received signal SIGSEGV 
>------------------------------------------------------<
wanghonglu  18:14
>---------------------- Row   4 -----------------------<
=> Start Application received signal SIGSEGV 
	-> translating『 0x1012891bc 』=> 
	-> translating『 0x100abf734 』=> __64-[RCTJSCExecutor executeApplicationScript:sourceURL:onComplete:]_block_invoke /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 515
	-> translating『 0x100abf944 』=> -[RCTJSCExecutor executeBlockOnJavaScriptQueue:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 547
	-> translating『 0x100abdc3c 』=> +[RCTJSCExecutor runRunLoopThread] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 151
=> End Application received signal SIGSEGV
wanghonglu  18:14
>---------------------- Row   7 -----------------------<
=> Start SyntaxError: Unexpected end of script 
	-> translating『 0x100aa8004 』=> RCTFatal /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTAssert.m: line 120
	-> translating『 0x100ac2b60 』=> -[RCTBatchedBridge stopLoadingWithError:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 476
	-> translating『 0x1006d72fc 』=> main /Users/wanghonglu/Desktop/project/pregnancy-v2-ios/pregnancy/pregnancy/main.m: line 13
=> End SyntaxError: Unexpected end of script 
>------------------------------------------------------<


>---------------------- Row   8 -----------------------<
=> Start Unable to execute JS call: __fbBatchedBridge is undefined 
	-> translating『 0x100aa8004 』=> RCTFatal /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTAssert.m: line 120
	-> translating『 0x100ac2b60 』=> -[RCTBatchedBridge stopLoadingWithError:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 476
	-> translating『 0x1006d72fc 』=> main /Users/wanghonglu/Desktop/project/pregnancy-v2-ios/pregnancy/pregnancy/main.m: line 13
=> End Unable to execute JS call: __fbBatchedBridge is undefined 
>------------------------------------------------------<
wanghonglu  18:14
>---------------------- Row  11 -----------------------<
=> Start Application received signal SIGSEGV 
	-> translating『 0x1012891bc 』=> 
	-> translating『 0x100a92140 』=> __46-[RCTNetworking buildRequest:completionBlock:]_block_invoke /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/Libraries/Network/RCTNetworking.m: line 215
	-> translating『 0x100a923bc 』=> -[RCTNetworking processDataForHTTPQuery:callback:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/Libraries/Network/RCTNetworking.m: line 264
	-> translating『 0x100a91e2c 』=> -[RCTNetworking buildRequest:completionBlock:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/Libraries/Network/RCTNetworking.m: line 204
	-> translating『 0x100a940a8 』=> -[RCTNetworking sendRequest:responseSender:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/Libraries/Network/RCTNetworking.m: line 442
	-> translating『 0x100aa67ec 』=> -[RCTModuleMethod invokeWithBridge:module:arguments:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTModuleMethod.m: line 426
	-> translating『 0x100ac53f0 』=> -[RCTBatchedBridge _handleRequestNumber:moduleID:methodID:params:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 899
	-> translating『 0x100ac4de0 』=> __33-[RCTBatchedBridge handleBuffer:]_block_invoke.383 /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 848
=> End Application received signal SIGSEGV 
>------------------------------------------------------<
wanghonglu  18:15
>---------------------- Row  16 -----------------------<
=> Start Unable to execute JS call: __fbBatchedBridge is undefined 
	-> translating『 0xa3b4ef 』=> RCTFatal /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTAssert.m: line 120
	-> translating『 0xa52b37 』=> -[RCTBatchedBridge stopLoadingWithError:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 476
	-> translating『 0x68b883 』=> main /Users/wanghonglu/Desktop/project/pregnancy-v2-ios/pregnancy/pregnancy/main.m: line 13
=> End Unable to execute JS call: __fbBatchedBridge is undefined 
>------------------------------------------------------<
wanghonglu  18:15
>---------------------- Row  22 -----------------------<
=> Start Application received signal SIGSEGV 
	-> translating『 0x1012891bc 』=> 
	-> translating『 0x100ac1c30 』=> -[RCTBatchedBridge registerModuleForFrameUpdates:withModuleData:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 357
	-> translating『 0x100ac707c 』=> -[RCTModuleData finishSetupForInstance] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTModuleData.m: line 73
	-> translating『 0x100ac1a74 』=> -[RCTBatchedBridge initModules] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 249
	-> translating『 0x100ac02a8 』=> -[RCTBatchedBridge start] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 99
	-> translating『 0x100ac016c 』=> -[RCTBatchedBridge initWithParentBridge:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 68
	-> translating『 0x100ad5648 』=> -[RCTBridge setUp] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBridge.m: line 257
	-> translating『 0x100ad5068 』=> -[RCTBridge initWithDelegate:launchOptions:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBridge.m: line 156
	-> translating『 0x100071c60 』=> __36-[BBRNManager reactNativeBridgeInit]_block_invoke /Users/wanghonglu/Desktop/project/pregnancy-v2-ios/pregnancy/pregnancy/ReactNative/Utils/BBRNManager.m: line 56
	-> translating『 0x1006d72fc 』=> main /Users/wanghonglu/Desktop/project/pregnancy-v2-ios/pregnancy/pregnancy/main.m: line 13
=> End Application received signal SIGSEGV 
>------------------------------------------------------<
wanghonglu  18:15
>---------------------- Row  24 -----------------------<
=> Start Application received signal SIGABRT 
	-> translating『 0x1012891bc 』=> 
	-> translating『 0x100abf290 』=> __52-[RCTJSCExecutor _executeJSCall:arguments:callback:]_block_invoke /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 391
	-> translating『 0x100abf944 』=> -[RCTJSCExecutor executeBlockOnJavaScriptQueue:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 547
	-> translating『 0x100abeda4 』=> -[RCTJSCExecutor _executeJSCall:arguments:callback:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 385
	-> translating『 0x100abeb7c 』=> -[RCTJSCExecutor callFunctionOnModule:method:arguments:callback:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 368
	-> translating『 0x100ac43b0 』=> -[RCTBatchedBridge _actuallyInvokeAndProcessModule:method:arguments:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 747
	-> translating『 0x100ac3774 』=> __39-[RCTBatchedBridge enqueueJSCall:args:]_block_invoke /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 642
	-> translating『 0x100abf944 』=> -[RCTJSCExecutor executeBlockOnJavaScriptQueue:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 547
	-> translating『 0x100abdc3c 』=> +[RCTJSCExecutor runRunLoopThread] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 151
=> End Application received signal SIGABRT 
>------------------------------------------------------<
wanghonglu  18:15
>---------------------- Row  25 -----------------------<
=> Start Application received signal SIGABRT 
	-> translating『 0x1012891bc 』=> 
	-> translating『 0x100ace79c 』=> _RCTJSONStringifyNoRetry /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTUtils.m: line 26
	-> translating『 0x100ace5bc 』=> RCTJSONStringify /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTUtils.m: line 71
	-> translating『 0x100abeeec 』=> __52-[RCTJSCExecutor _executeJSCall:arguments:callback:]_block_invoke /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 391
	-> translating『 0x100abf944 』=> -[RCTJSCExecutor executeBlockOnJavaScriptQueue:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 547
	-> translating『 0x100abeda4 』=> -[RCTJSCExecutor _executeJSCall:arguments:callback:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 385
	-> translating『 0x100abeb7c 』=> -[RCTJSCExecutor callFunctionOnModule:method:arguments:callback:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 368
	-> translating『 0x100ac43b0 』=> -[RCTBatchedBridge _actuallyInvokeAndProcessModule:method:arguments:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 747
	-> translating『 0x100ac3774 』=> __39-[RCTBatchedBridge enqueueJSCall:args:]_block_invoke /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 642
	-> translating『 0x100abf944 』=> -[RCTJSCExecutor executeBlockOnJavaScriptQueue:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 547
	-> translating『 0x100abdc3c 』=> +[RCTJSCExecutor runRunLoopThread] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Executors/RCTJSCExecutor.m: line 151
=> End Application received signal SIGABRT 
>------------------------------------------------------<
wanghonglu  18:16
>---------------------- Row  26 -----------------------<
=> Start Application received signal SIGSEGV 
	-> translating『 0x1012891bc 』=> 
	-> translating『 0x100ac1c30 』=> -[RCTBatchedBridge registerModuleForFrameUpdates:withModuleData:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 357
	-> translating『 0x100ac707c 』=> -[RCTModuleData finishSetupForInstance] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTModuleData.m: line 73
	-> translating『 0x100ac7400 』=> -[RCTModuleData instance] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTModuleData.m: line 122
	-> translating『 0x100ac113c 』=> -[RCTBatchedBridge moduleForName:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 231
	-> translating『 0x100ad540c 』=> -[RCTBridge moduleForClass:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBridge.m: line 236
	-> translating『 0x100ae16d4 』=> -[RCTUIManager setBridge:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Modules/RCTUIManager.m: line 270
	-> translating『 0x100ac6f2c 』=> -[RCTModuleData setBridgeForInstance] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTModuleData.m: line 58
	-> translating『 0x100ac19b0 』=> -[RCTBatchedBridge initModules] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 249
	-> translating『 0x100ac02a8 』=> -[RCTBatchedBridge start] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 99
	-> translating『 0x100ac016c 』=> -[RCTBatchedBridge initWithParentBridge:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 68
	-> translating『 0x100ad5648 』=> -[RCTBridge setUp] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBridge.m: line 257
	-> translating『 0x100ad5068 』=> -[RCTBridge initWithDelegate:launchOptions:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBridge.m: line 156
	-> translating『 0x100071c60 』=> __36-[BBRNManager reactNativeBridgeInit]_block_invoke /Users/wanghonglu/Desktop/project/pregnancy-v2-ios/pregnancy/pregnancy/ReactNative/Utils/BBRNManager.m: line 56
	-> translating『 0x1006d72fc 』=> main /Users/wanghonglu/Desktop/project/pregnancy-v2-ios/pregnancy/pregnancy/main.m: line 13
=> End Application received signal SIGSEGV 
>------------------------------------------------------<
wanghonglu  18:16
>---------------------- Row  27 -----------------------<
=> Start SyntaxError: Unexpected EOF 
	-> translating『 0x100aa8004 』=> RCTFatal /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTAssert.m: line 120
	-> translating『 0x100ac2b60 』=> -[RCTBatchedBridge stopLoadingWithError:] /Users/kevin/Code/pregnancy-rn/mobile-rn/node_modules/react-native/React/Base/RCTBatchedBridge.m: line 476
	-> translating『 0x1006d72fc 』=> main /Users/wanghonglu/Desktop/project/pregnancy-v2-ios/pregnancy/pregnancy/main.m: line 13
=> End SyntaxError: Unexpected EOF 
>------------------------------------------------------<
```

# __fbBatchedBridge is undefined 3次；Unexpected end of script 1次；Unexpected EOF 1次；

场景：异常从RCTBatchedBridge stopLoadingWithError方法【此方法是当jsbundle加载出现异常时就会执行】抛出，应为加载的jsbundle文件有问题，有可能是文件下载不对或存储空间不足等，建议加载前，使用md5进行校验。

解决：建议加载前，使用md5进行校验。校验不通过的不进行加载，尝试回退or重新下载补丁等动作。

android端也出现了__fbBatchedBridge is undefined的异常，解法也是使用md5来校验jsbundle。

参考链接：http://www.jianshu.com/p/3db03c8c4ae7


# Start Application received signal SIGSEGV

## [RCTJSCExecutor executeApplicationScript:sourceURL:onComplete:]2次

-> translating『 0x1012891bc 』=> 

最根的没有转换出来

RCTJSCExecutor executeApplicationScrip

RCTJSCExecutor executeBlockOnJavaScriptQueue

场景：暂时没有线索

解决： todo


## [RCTNetworking buildRequest:completionBlock:] 1次

-> translating『 0x1012891bc 』=> 

最根的没有转换出来

场景：js调用原生，发送网络请求

解决：todo

## [RCTBatchedBridge registerModuleForFrameUpdates:withModuleData:] 2次

-> translating『 0x1012891bc 』=> 

最根的没有转换出来

场景：初始化bridge的过程，[BBRNManager reactNativeBridgeInit]中调用[RCTBridge initWithDelegate:launchOptions:]的时候。

解决：todo

# Start Application received signal SIGABRT

## [RCTJSCExecutor _executeJSCall:arguments:callback:] 1次

-> translating『 0x1012891bc 』=> 

最根的没有转换出来

场景：是由RCTBatchedBridge enqueueJSCall调用，是native调用js。

解决：todo

## RCTUtils.m 中的 _RCTJSONStringifyNoRetry 1次

-> translating『 0x1012891bc 』=> 

最根的没有转换出来

场景：将json对象转换为json字符串。也是由[RCTJSCExecutor _executeJSCall:arguments:callback:]调用造成的。是由RCTBatchedBridge enqueueJSCall调用，是native调用js。

解决：todo

