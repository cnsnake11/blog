#Immutable.js的问题


##容易与原生对象混淆

参考地址：http://zhuanlan.zhihu.com/purerender/20295971

这点是我们使用 Immutable.js 过程中遇到最大的问题。写代码要做思维上的转变。

虽然 Immutable.js 尽量尝试把 API 设计的原生对象类似，有的时候还是很难区别到底是 Immutable 对象还是原生对象，容易混淆操作。

Immutable 中的 Map 和 List 虽对应原生 Object 和 Array，但操作非常不同，比如你要用 `map.get('key')` 而不是 `map.key`，`array.get(0)` 而不是 `array[0]`。另外 Immutable 每次修改都会返回新对象，也很容易忘记赋值。

当使用外部库的时候，一般需要使用原生对象，也很容易忘记转换。

下面给出一些办法来避免类似问题发生：

1. 使用 Flow 或 TypeScript 这类有静态类型检查的工具约定变量命名规则：
1. 如所有 Immutable 类型对象以 `$$` 开头。
2. 使用 `Immutable.fromJS` 而不是 `Immutable.Map` 或 `Immutable.List` 来创建对象，这样可以避免 Immutable 和原生对象间的混用。


##tips 有时候还原回普通对象进行批量操作更方便

