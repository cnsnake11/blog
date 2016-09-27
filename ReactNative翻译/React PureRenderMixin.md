# React PureRenderMixin

英文原文地址：https://facebook.github.io/react/docs/pure-render-mixin.html

If your React component's render function is "pure" (in other words, it renders the same result given the same props and state), you can use this mixin for a performance boost in some cases.

如果你的react组件是纯粹的（换句话说，如果传入相同的props和state，每次都将渲染相同的结果），在某些情况下，你就可以使用PureRenderMixin来提升性能。

Example:

```
var PureRenderMixin = require('react-addons-pure-render-mixin');
React.createClass({
  mixins: [PureRenderMixin],

  render: function() {
    return <div className={this.props.className}>foo</div>;
  }
});
```

Example using ES6 class syntax:

```
import PureRenderMixin from 'react-addons-pure-render-mixin';
class FooComponent extends React.Component {
  constructor(props) {
    super(props);
    this.shouldComponentUpdate = PureRenderMixin.shouldComponentUpdate.bind(this);
  }

  render() {
    return <div className={this.props.className}>foo</div>;
  }
}
```

Under the hood, the mixin implements shouldComponentUpdate, in which it compares the current props and state with the next ones and returns false if the equalities pass.

深入来讲，这个mixin实现了shouldComponentUpdate方法，它对当前props、state和即将要使用的props、state进行了比较，如果一致就返回false，有不相等项就返回true。

Note:
This only shallowly compares the objects. If these contain complex data structures, it may produce false-negatives for deeper differences. Only mix into components which have simple props and state, or use forceUpdate() when you know deep data structures have changed. Or, consider using immutable objects to facilitate fast comparisons of nested data.
Furthermore, shouldComponentUpdate skips updates for the whole component subtree. Make sure all the children components are also "pure".

注意：
这个方案仅仅对object进行了浅比对。如果是复杂数据类型，可能会出现错误。请将此mixin应用到使用了简单数据类型props和state的组件，或者当复杂数据类型发生变化时调用forceUpdate()方法。还有一个更好更快的方案，使用不可变数据类型。
shouldComponentUpdate会忽略掉整个组件树的更新操作，确保所有的子组件都是纯粹的。


