# Range

在做富文本编辑器的开发过程中，Selection和Range对象是用来操作选中区域的办法，本篇主要介绍一下Range对象的常用接口和属性。

# 介绍

Range可以用 Document 对象的 createRange方法创建，也可以用Selection对象的getRangeAt方法取得。另外，可以通过构造函数 Range() 来获得一个 Range 。


# 属性

#### Range.collapsed 只读
返回一个用于判断 Range 起始位置和终止位置是否相同的布尔值。
#### Range.commonAncestorContainer 只读
返回包含 startContainer 和 endContainer 的最深的节点。
#### Range.endContainer 只读
返回包含 Range 终点的节点。
#### Range.endOffset 只读
返回 endContainer 中表示Range终点位置的数字。
#### Range.startContainer 只读
返回包含 Range 开始的节点。
#### Range.startOffset 只读
返回 startContainer 中表示 Range 起始位置的数字。

# 方法

#### Range.setStart()
设置 Range 的起点。
#### Range.setEnd()
设置 Range 的终点。
#### Range.setStartBefore()
以其它节点 （ Node）为基准，设置 Range 的起点。
#### Range.setStartAfter()
以其它节点为基准，设置 Range 的始点。
#### Range.setEndBefore()
以其它节点为基准，设置 Range 的终点。
#### Range.setEndAfter()
以其它节点为基准，设置 Range 的终点。
#### Range.selectNode()
设定一个包含节点和节点内容的 Range 。
#### Range.selectNodeContents()
设定一个包含某个节点内容的 Range 。
#### Range.collapse()
向指定端点折叠该 Range 。


#### Range.cloneContents()
返回 Range 当中节点的文档片段（DocumentFragment）。
#### Range.deleteContents()
从文档（Document）中移除 Range 中的内容。
#### Range.extractContents()
把 Range 的内容从文档树移动到文档片段中。
#### Range.insertNode()
在 Range 的起点处插入节点。
#### Range.surroundContents()
将 Range 的内容移动到一个新的节点中。


#### Range.compareBoundaryPoints()
比较两个 Range 的端点。
#### Range.cloneRange()
返回拥有和原 Range 相同端点的克隆 Range 对象。
#### Range.detach()
从使用状态释放 Range，此方法用于改善性能。
#### Range.toString()
把Range内容作为字符串返回。



