# 前端应该掌握的一些app开发知识-ios篇（1）

移动端混合开发是各大公司非常常见的开发模式，作为前端能了解一些原生开发的基础知识，对程序设计，问题分析都会有很大的益处。

此系列文章对ios原生app开发的一些基础知识做出了归纳和总结，方便前端的同学快速的了解ios开发的知识。

本文主要对知识点进行归纳和总结，不做细节的描述。

在ios的第一篇中，我们先总结一下object-c。

# IDE

编写OC程序最主要的编译环境是Xcode,它是苹果官方提供的IDE,官网中的SDK包括Xcode,可以通过下载SDK来获得它。但是Xcode只支持MacOSX,所以如果要在其它环境下编写OC程序,要使用其它IDE。Linux/FreeBSD用GNUStep,Windows NT5.x(2000,XP)要先安装cywin或mingw,然后安装GNUStep。同时仅仅通过文本编辑器,GCC的make工具也可以用于开发。
注:如果要使用到Cocoa的话，只能在Apple公司的Xcode上

# 框架

OC编程中主要用到的框架是Cocoa,它是MacOS X中五大API之一,它由两个不同的框架组成FoundationKit 和ApplicationKit。Foundation框架拥有100多个类,其中有很多有用的、面向数据的低级类和数据类型,如NSString,NSArray,NSEnumerator和NSNumber。ApplicationKit包含了所有的用户接口对象和高级类。这些框架本文不做重点介绍,如果要深入了解可以去看Xcode自带的文档。

# object-c

object-c通常写作objective-c或者obj-c，是根据C语言所衍生出来的语言，继承了C语言的特性，是扩充C的面向对象编程语言。它主要使用于Mac OS X。在MAC OSX系统下，运用苹果提供的SDK等开发工具包，可以用来做IOS开发，开发后的程序在Iphone虚拟机中进行测试，运用的主要语言为Object-c。

# swift

Swift，苹果于2014年WWDC(苹果开发者大会)发布的新开发语言，可与Objective-C*共同运行于Mac OS和iOS平台，用于搭建基于苹果平台的应用程序。

Swift是苹果公司在WWDC2014上发布的全新开发语言。从演示视频及随后在appstore上线的标准文档看来，语法内容混合了OC,JS,Python，语法简单，使用方便，并可与OC混合使用。

Xcode目前已有1400万次下载量，而全新的编程语言Swift改变了Obejective-C复杂的语法，并保留了Smalltalk的动态特性，简而言之就是敏捷易用，大家都说苹果的生态圈要优于Google，现在苹果又进一步完善了开发生态圈。


# object-c or swift

oc的优势是使用时间久，轮子多，社区成熟度高，缺点是语法过于复杂，不易人类阅读。

swift的优缺点正好和oc相反。

Swift是未来的趋势，但是OC还是很重要，因为很多公司的项目都是基于oc的。


# 反人类之处

初次接触OC时,会发现许多和其它语言不同的地方,会看到很多的+,-,[ ,] ,@,NS等符号。

例如，反人类的字符串写法是以@开头，@“hello”

# 扩展名

OC是ANSI版本C的一个超集,它支持相同的C语言基本语法。与C一样,文件分为头文件和源文件,扩展名分别为.h和.m。

.h  头文件。头文件包涵类的定义、类型、方法以及常量的声明
        
.m  源文件。这个典型的扩展名用来定义源文件，可以同时包含C和Objective-C的代码。

# #import

在OC里,包含头文件有比#include更好的方法#import。它的使用和#include相同,并且可以保证你的程序只包含相同的头文件一次。相当于#include+ #pragma once的组合。每个框架有一个主的头文件,只要包含了这个文件,框架中的所有特性都可以被使用。

# @符号

@符号是OC在C基础上新加的特性之一。常见到的形式有@”字符串”,%@ , @interface,@implement等。@”字符串”表示引用的字符串应该作为Cocoa的NSString元素来处理。@interface等则是对于C的扩展,是OC面向对象特性的体现。

# NSLog()

在OC中用的打印函数是NSLog(),因为OC是加了一点”特殊语料”的C语言,所以也可以用printf()但是NSLog()提供了一些特性,如时间戳,日期戳和自动加换行符等,用起来更方便,所以推荐使用NSLog()。

# id

这是OC新加的一个数据类型,它是一般的对象类型,能够存储任何类型的方法。

# nil

在OC中,相对于C中的NULL,用的是nil。这两者是等价的。

# 定义方法

在OC中一个类中的方法有两种类型：实例方法，类方法。实例方法前用(-)号表明,类方法用(+)表明。

