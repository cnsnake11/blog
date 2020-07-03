# React Shallow Compare

英文原文地址：https://facebook.github.io/react/docs/shallow-compare.html

shallowCompare is a helper function to achieve the same functionality as PureRenderMixin while using ES6 classes with React.

shallowCompare是用es6方式来实现与PureRenderMixin一样的功能的函数。

If your React component's render function is "pure" (in other words, it renders the same result given the same props and state), you can use this helper function for a performance boost in some cases.

如果你的react组件是纯粹的（换句话说，如果传入相同的props和state，每次都将渲染相同的结果），在某些情况下，你就可以使用shallowCompare来提升性能。

Example:

```
var shallowCompare = require('react-addons-shallow-compare');
export class SampleComponent extends React.Component {
  shouldComponentUpdate(nextProps, nextState) {
    return shallowCompare(this, nextProps, nextState);
  }

  render() {
    return <div className={this.props.className}>foo</div>;
  }
}
```

shallowCompare performs a shallow equality check on the current props and nextProps objects as well as the current state and nextState objects.
It does this by iterating on the keys of the objects being compared and returning true when the values of a key in each object are not strictly equal.

shallowCompare在当前props和未来props（当前state和未来state）进行了浅比对。过程是遍历object的所有属性值并进行===的判断，当有不相等项的时候就返回true。

shallowCompare returns true if the shallow comparison for props or state fails and therefore the component should update.
shallowCompare returns false if the shallow comparison for props and state both pass and therefore the component does not need to update.

shallowCompare返回true当props或state有不相等项，此时组件将会更新。
shallowCompare返回true当props和state的所有项目都相等，此时组件不需要更新。


