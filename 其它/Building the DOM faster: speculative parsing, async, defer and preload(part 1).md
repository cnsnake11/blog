# 【翻译】Building the DOM faster: speculative parsing, async, defer and preload (part 1)

# 更快的dom渲染：预解析，异步，延迟和提前加载 ~ 第一部分

原文地址：https://hacks.mozilla.org/2017/09/building-the-dom-faster-speculative-parsing-async-defer-and-preload/

In 2017, the toolbox for making sure your web page loads fast includes everything from minification and asset optimization to caching, CDNs, code splitting and tree shaking. However, you can get big performance boosts with just a few keywords and mindful code structuring, even if you’re not yet familiar with the concepts above and you’re not sure how to get started.

优化网页打开速度的办法有很多，比如，压缩静态资源文件，使用缓存，使用cdn，代码拆分等。然而，尽管你可能不了解上述的办法，你也可以通过一些少量的代码和特定的代码结构来大幅提升网页的性能。

The fresh web standard ``` <link rel="preload"> ```, that allows you to load critical resources faster, is coming to Firefox later this month. You can already try it out in Firefox Nightly or Developer Edition, and in the meantime, this is a great chance to review some fundamentals and dive deeper into performance associated with parsing the DOM.

最新的web标准提供了``` <link rel="preload"> ```的写法，可以更快的加载特定的资源，这个特性在FIrefox中即将支持。现在，可以在firefox的nightly版本或者开发者版本中试用，同时在试用的时候，我们可以深入研究一下浏览器解析dom的细节。

Understanding what goes on inside a browser is the most powerful tool for every web developer. We’ll look at how browsers interpret your code and how they help you load pages faster with speculative parsing. We’ll break down how defer and async work and how you can leverage the new keywordpreload.

其实，对浏览器渲染原理的深入理解才是web开发者最强大的武器。在这篇文章中，将会介绍浏览器是如何解析代码，并且是如何使用预解析来提升页面的加载速度的。

# Building blocks

HTML describes the structure of a web page. To make any sense of the HTML, browsers first have to convert it into a format they understand – the Document Object Model, or DOM. Browser engines have a special piece of code called a parser that’s used to convert data from one format to another. An HTML parser converts data from HTML into the DOM.

In HTML, nesting defines the parent-child relationships between different tags. In the DOM, objects are linked in a tree data structure capturing those relationships. Each HTML tag is represented by a node of the tree (a DOM node).

The browser builds up the DOM bit by bit. As soon as the first chunks of code come in, it starts parsing the HTML, adding nodes to the tree structure.

![](media/15118651219648.gif)

The DOM has two roles: it is the object representation of the HTML document, and it acts as an interface connecting the page to the outside world, like JavaScript. When you call document.getElementById(), the element that is returned is a DOM node. Each DOM node has many functions you can use to access and change it, and what the user sees changes accordingly.



