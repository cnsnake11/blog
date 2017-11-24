# 【翻译】Building the DOM faster: speculative parsing, async, defer and preload (part 1)

# 

原文地址：https://hacks.mozilla.org/2017/09/building-the-dom-faster-speculative-parsing-async-defer-and-preload/

In 2017, the toolbox for making sure your web page loads fast includes everything from minification and asset optimization to caching, CDNs, code splitting and tree shaking. However, you can get big performance boosts with just a few keywords and mindful code structuring, even if you’re not yet familiar with the concepts above and you’re not sure how to get started.

The fresh web standard ``` <link rel="preload"> ```, that allows you to load critical resources faster, is coming to Firefox later this month. You can already try it out in Firefox Nightly or Developer Edition, and in the meantime, this is a great chance to review some fundamentals and dive deeper into performance associated with parsing the DOM.

Understanding what goes on inside a browser is the most powerful tool for every web developer. We’ll look at how browsers interpret your code and how they help you load pages faster with speculative parsing. We’ll break down howdefer and async work and how you can leverage the new keywordpreload.

# Building blocks



