#ReactNative增量升级方案

#前言
facebook的react-native给我们带来了用js写出原生应用的同时，也使得使用RN编写的代码的在线升级变得可能，终于可以不通过应用市场来进行升级，极大的提升了app修bug和赋予新功能的能力。----使用h5的方式也可以做到，但是rn的用户体验可要远远超过h5啊。

一般使用RN编写的app的线上使用方式，会使用react-native bundle命令打出bundle文件和assets文件夹，然后内置到app中，app在viewcontroller或者activity中直接加载app内部的bundle文件。

当修改了代码或者图片的时候，只要app使用新的bundle文件和assets文件夹，就完成了一次在线升级。

本文主要基于以上思路，讲解增量升级的解决方案。

#何为增量？



#系统结构设计与各模块职责
![312313123](media/312313123.png)


#bundle仓库设计

#升级服务器设计

#native客户端设计

