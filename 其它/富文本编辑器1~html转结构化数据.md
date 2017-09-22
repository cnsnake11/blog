# 富文本编辑器1~html转结构化数据

# 行业背景

在目前的互联网环境下，能源源不断产生优质的内容的平台，一定会受到用户的喜爱。

那么如何能让作者们快速产生优质的内容是内容平台一个不可或缺的功能。

一般来说，会使用一个叫富文本编辑器的东东，它大概长成这样：

![](media/15060671959019.jpg)

通过它，作者们就能快速的编写自己的文章，可以进行排版，设置字体样式，插入图片、视频等等内容，让文章美观漂亮内容丰富。

本系列文章会介绍我们在富文本编辑器开发过程中遇到的各种问题及其解决方案。

# 业务描述

富文本编辑器最终产生的是一段html字符串，这段字符串里包含了排版，样式，等信息。

如果把这个字符串直接拿到浏览器【pc浏览器，手机浏览器，手机webview均可】中渲染，不费吹灰之力就会原封不动的展现出来，可以说是省时省力省心。

但现实情况往往不这么理想。。。o(╯□╰)o

公司项目的显示终端有：pc网站，手机端网站，手机端app中，网站都还好是用浏览器来渲染的，无需操心。但手机app中显示内容的时候并没有用webview作为载体，而是原生代码根据json格式的结构化数据来渲染的。比如这样的json数据：

```

[
{
	content:'文字文字文字',
	type: '文字'
},
{
	url:'http://图片地址',
	type: '图片'
},
{
	url:'http://视频地址',
	cover: 'http://视频封面地址',
	title: '视频标题',
	type: '视频'
}
]

```

而富文本编辑器生成的html字符串是这样的：

```

<p>段落1段落1段落1段落1段落1段落1段落1段落1段落1<img src="http://pic09.babytreeimg.com/contentplatform/20170922/FueIRKRv3t6fFQa0kBr8sg5caEQA" _bbt_json="{&quot;t&quot;:&quot;2&quot;,&quot;id&quot;:&quot;4962&quot;,&quot;path&quot;:&quot;http://pic09.babytreeimg.com/contentplatform/20170922/FueIRKRv3t6fFQa0kBr8sg5caEQA&quot;}">段落2段落2段落2<b>段落2段落2段落2段落2段<i>落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落</i></b><i>2段落2段落2段落</i>2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2段落2</p><br><p><img src="http://pic06.babytreeimg.com/contentplatform/20170922/FmSyH6SL_8BD5TYIWERjOqjwPcZ9" _bbt_json="{&quot;t&quot;:&quot;2&quot;,&quot;id&quot;:&quot;4960&quot;,&quot;path&quot;:&quot;http://pic06.babytreeimg.com/contentplatform/20170922/FmSyH6SL_8BD5TYIWERjOqjwPcZ9&quot;}"><br></p><p>段落3段落3段落3段落3段落3段落3段落3段落3段落3段落3</p>

```

所以，就需要一个转换引擎，来将html字符串转换为结构化的数据。

# 难点

比如我们如下的html：

```

<h1>
     <p>
             <i>
                 我是文本1
                 <img src="地址" />
                 我是<u>文本</u>2
                 </i>
             </p>
     </h1>

```

拆成结构化json，应该是这样的：

```

[
{
	content:'<h1><p><i>我是文本1</i></p></h1>',
	type: '文字'
},
{
	url:'http://图片地址',
	type: '图片'
},
{
	content:'<h1><p><i>我是<u>文本</u>2</i></p></h1>',
	type: '文字'
}
]

```

仔细看一下就会发现，除了执行字符串分割之外，还要进行html开始标签和结束标签的补位。

文本1需要对html结束标签进行补位，也就是</i></p></h1>。

文本2需要对html开始标签进行补位，也就是<h1><p><i>。

# 算法原理

一开始，想直接对html字符串的解析工作来把补位标签计算出来，发现实现的复杂度有点高，而且对字符串编程的代码可读性也比较差。

后来突然想到，html字符串本身就是一个dom树形结构，可不可以利用一下dom树的接口来辅助进行解析呢。

所以整个算法主要分成2步骤。

### 步骤1 利用dom树接口，分析出中间结果

上面例子中的中间结果应该是这样一个对象的数组：

```

[
{
	type: 图片,
	src: '图片地址',
	startTag: '<h1><p><i>',
	endTag: '</i></p></h1>'
}
]

```
如何分析出这样的中间结果呢？

首先，对dom树进行递归遍历，遍历到图片的el的时候，在对图片el不停的获取parentNode，直到需要解析的dom容器的顶端，按顺序记录下parentNode的tag名称，就是我们要的结果。比如如下的代码：

```

getTags = (el) => {     let el2 = el;     let start = '';     let end = '';     while (true) {         el2 = el2.parentNode;         if (el2 === this.container) {             return { start, end };         }         if (el2 && el2.tagName) {             end = `${end}</${el2.tagName.toLowerCase()}>`;             start = `<${el2.tagName.toLowerCase()}>${start}`;         }     } }

```
所以，实际上我们是利用了dom树的childNodes接口进行递归遍历，找到目标元素之后，又利用dom树的parentNode接口进行向上查找。

### 步骤2 利用中间结果，分割字符串，补齐html标签

有了中间结果数组之后，后面就简单了，遍历中间结果数组，针对数组的每一项，找到html字符串中对应的字符串【比如<img src="地址" />】，将其前面的部分分割出来【也就是<h1><p><i>我是文本1】，并在其后面补上中间结果中的endTag【</i></p></h1>】，然后将中间结果后面的字符串【我是<u>文本</u>2</i></p></h1>】加上startTag【<i><p><h1>】，然后继续遍历即可。代码如下：

```

processHTML = () => {     let html = this.container.innerHTML;     if (this.domResult.length === 0) {         this.result.push({             t: '1',             txt: html,         });         return;     }     for (let i = 0; i < this.domResult.length; i += 1) {         const one = this.domResult[i];         const index = html.indexOf(one.html);         const txt = html.substring(0, index) + one.tags.end;          this.result.push({             t: '1',             txt,         });         this.result.push(one.json);          html = html.substring(index + one.html.length, html.length);         html = one.tags.start + html;          if (i === this.domResult.length - 1 && html) {             this.result.push({                 t: '1',                 txt: html,             });         }     } }

```

# 完整代码

```

import { BBT_JSON } from './Constant';  export default class HtmlParser {      constructor(container) {         this.container = container; // 容器对象         this.domResult = []; // 解析dom的结果 用于替换字符串用         this.result = []; // 最终的运行结果     }      getData = () => {         // 1 遍历dom树 解析需要替换的节点和替换的内容 使用标记标签替换原标签         this.processDom(this.container);          // 2 执行html字符串的分割和标签补齐 并生成格式化数据         this.processHTML();          return this.result;     }      processDom = (root) => {         const childs = root.childNodes;         for (let i = 0; i < childs.length; i += 1) {             const el = childs[i];             if (el.getAttribute && el.getAttribute(BBT_JSON)) {                 const tags = this.getTags(el);                 this.domResult.push({                     html: el.outerHTML,                     json: JSON.parse(el.getAttribute(BBT_JSON)),                     tags,                 });             }             this.processDom(el);         }     }      processHTML = () => {         let html = this.container.innerHTML;         if (this.domResult.length === 0) {             this.result.push({                 t: '1',                 txt: html,             });             return;         }         for (let i = 0; i < this.domResult.length; i += 1) {             const one = this.domResult[i];             const index = html.indexOf(one.html);             const txt = html.substring(0, index) + one.tags.end;              this.result.push({                 t: '1',                 txt,             });             this.result.push(one.json);              html = html.substring(index + one.html.length, html.length);             html = one.tags.start + html;              if (i === this.domResult.length - 1 && html) {                 this.result.push({                     t: '1',                     txt: html,                 });             }         }     }      getTags = (el) => {         let el2 = el;         let start = '';         let end = '';         while (true) {             el2 = el2.parentNode;             if (el2 === this.container) {                 return { start, end };             }             if (el2 && el2.tagName) {                 end = `${end}</${el2.tagName.toLowerCase()}>`;                 start = `<${el2.tagName.toLowerCase()}>${start}`;             }         }     } } 

```

