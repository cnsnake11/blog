# ios11和iPhone X中Webview的适配.md

原理和原因请参考：

ios11的webview的默认渲染页面的方式发生了变化，不再和ios7~ios10一致：

ios7~ios10：会将页面铺满整个屏幕，也就相当于是viewport-fit=cover。

ios11：默认将页面铺满到安全区域中，非iphoneX就是顶部状态条之下，iPhone X就是留海之下开始。也就是相当于viewport-fit=contain。

所以，有3种情况要适配，1是适配ios11的非iPhone X手机，2是适配iPhone X手机，3是其它的情况。



