# 【翻译】Reconciliation React比对算法

原文地址：https://facebook.github.io/react/docs/reconciliation.html

React provides a declarative API so that you don't have to worry about exactly what changes on every update. This makes writing applications a lot easier, but it might not be obvious how this is implemented within React. This article explains the choices we made in React's "diffing" algorithm so that component updates are predictable while being fast enough for high-performance apps.

使用react你就不用去关注每次更新UI的时候真正更新了哪些节点。这样的话开发业务代码会非常简单，但并不能理解react的实现原理。本文会解释react的比对算法，理解了这个算法，会让你更容易构建高性能的应用。

# Motivation 动机

When you use React, at a single point in time you can think of the render() function as creating a tree of React elements. On the next state or props update, that render() function will return a different tree of React elements. React then needs to figure out how to efficiently update the UI to match the most recent tree.

在某一时刻，react的render方法会返回一组树形节点。当state发生变化，render方法会返回一组不同的树形节点。react需要分析出如何更新UI是最高效、损耗最小的。

There are some generic solutions to this algorithmic problem of generating the minimum number of operations to transform one tree into another. However, the state of the art algorithms have a complexity in the order of O(n3) where n is the number of elements in the tree.

会有几种算法来保证最小化的更新操作。然而，这些算法都还是有O(n3)的复杂度，n是节点的数量，n3是n的3次方。

If we used this in React, displaying 1000 elements would require in the order of one billion comparisons. This is far too expensive. Instead, React implements a heuristic O(n) algorithm based on two assumptions:

1. Two elements of different types will produce different trees.
2. The developer can hint at which child elements may be stable across different renders with a key prop.

In practice, these assumptions are valid for almost all practical use cases.

也就是说，如果有1000个节点，那就要比对1000000000次。这损耗太高了。所以，react实现了新的O(n)算法，但是需要满足如下2个假设：

1. 2个不同类型的节点，会产生不同的UI树
2. 开发者可以提供一个叫key的属性，来指出是否为同一个节点

在实际应用中，这2个假设都是普遍成立的。

# The Diffing Algorithm 比对算法

When diffing two trees, React first compares the two root elements. The behavior is different depending on the types of the root elements.

react比对2棵树的时候，会首先比对根节点。会根据根节点的类型来比对。

### Elements Of Different Types 不同类型的节点

Whenever the root elements have different types, React will tear down the old tree and build the new tree from scratch. Going from a to img, or from Article to Comment, or from Button to div - any of those will lead to a full rebuild.

当根节点是不同类型的时候，react将会移除整棵老树，完全新建一棵树替代。比如，a to img或者from Article to Comment, 或者 from Button to div，所有这些改变都会触发一次完整的重新构建动作。

When tearing down a tree, old DOM nodes are destroyed. Component instances receive componentWillUnmount(). When building up a new tree, new DOM nodes are inserted into the DOM. Component instances receive componentWillMount() and then componentDidMount(). Any state associated with the old tree is lost.

在移除老树的过程中，所有的dom节点都会被销毁。根组件实例会触发componentWillUnmount事件。当新建一棵新树，新的dom节点会别插入到dom中。根组件实例会触发componentWillMount及componentDidMount。所有老树的state也会被销毁。

Any components below the root will also get unmounted and have their state destroyed. For example, when diffing:

老树的所有孩子节点也会被完全的销毁，同时他们的state也会被销毁。比如下面的例子:

```
<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```

This will destroy the old Counter and remount a new one.

老的Counter会被销毁，会新建一个Counter替代。

### DOM Elements Of The Same Type 相同类型的dom节点

When comparing two React DOM elements of the same type, React looks at the attributes of both, keeps the same underlying DOM node, and only updates the changed attributes. For example:

在比较相同类型的节点的时候，react会比较两个节点的属性，保持dom节点对象不变，然后会更新值发生改变的属性。例如：

```
<div className="before" title="stuff" />

<div className="after" title="stuff" />
```

By comparing these two elements, React knows to only modify the className on the underlying DOM node.

react比对这2个节点后，只会更新className。

When updating style, React also knows to update only the properties that changed. For example:

当更新样式的时候，react也只更新改变的属性。例如:

```
<div style={{color: 'red', fontWeight: 'bold'}} />

<div style={{color: 'green', fontWeight: 'bold'}} />

```

When converting between these two elements, React knows to only modify the color style, not the fontWeight.


针对这2个节点，react会只更新color，而不会更新fontWeight。

After handling the DOM node, React then recurses on the children.

当比对完一个节点后，react会继续递归这个节点的孩子节点。

### Component Elements Of The Same Type 相同类型的component组件节点

When a component updates, the instance stays the same, so that state is maintained across renders. React updates the props of the underlying component instance to match the new element, and calls componentWillReceiveProps() and componentWillUpdate() on the underlying instance.

当组件节点更新，会保持同一个对象实例，state会保持不变。react会更新那些改变过组件节点的属性，并且触发他们的componentWillReceiveProps和componentWillUpdate。

Next, the render() method is called and the diff algorithm recurses on the previous result and the new result.

下一次改变发生时，整个流程会再次执行。

### Recursing On Children 递归孩子节点

By default, when recursing on the children of a DOM node, React just iterates over both lists of children at the same time and generates a mutation whenever there's a difference.

默认情况下，在递归孩子节点的时候，react会同时遍历新老树的孩子节点，并标记处出改变的地方。

For example, when adding an element at the end of the children, converting between these two trees works well:

举个栗子，在某个节点的末尾插入一个新节点，比对算法会高效的执行：

```
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```

React will match the two li first trees, match the two li second trees, and then insert the li third tree.

react会认为2个li first是相同的，2个li second是相同的，并找出了新增的li third。

If you implement it naively, inserting an element at the beginning has worse performance. For example, converting between these two trees works poorly:

但是，如果在某个几点的开头插入一个节点，比对算法会执行的很低效，例如：

```
<ul>
  <li>Duke</li>
  <li>Villanova</li>
</ul>

<ul>
  <li>Connecticut</li>
  <li>Duke</li>
  <li>Villanova</li>
</ul>
```

React will mutate every child instead of realizing it can keep the li Duke and li Villanova subtrees intact. This inefficiency can be a problem.

react会认为每一个孩子节点都发生了变化，而不是保持li Duke和li Villanova不变。这种低效的执行很成问题。


### Keys

In order to solve this issue, React supports a key attribute. When children have keys, React uses the key to match children in the original tree with children in the subsequent tree. For example, adding a key to our inefficient example above can make the tree conversion efficient:

为了解决这个问题，react使用了key属性。当孩子节点有key属性的时候，react会使用key属性的值去匹配2棵树的孩子节点。例如，在我们上面低效的例子中，为孩子节点添加key属性如下：

```
<ul>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

<ul>
  <li key="2014">Connecticut</li>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>
```

Now React knows that the element with key '2014' is the new one, and the elements with the keys '2015' and '2016' have just moved.

这样的话，react就知道key是2014的是新增的，而2015和2016仅仅是移动了。

In practice, finding a key is usually not hard. The element you are going to display may already have a unique ID, so the key can just come from your data:

在实践中，使用key属性并不难。要显示的元素通常都有一个唯一的id，所以，使用这个id就可以了：

```
<li key={item.id}>{item.name}</li>
```

When that's not the case, you can add a new ID property to your model or hash some parts of the content to generate a key. The key only has to be unique among its siblings, not globally unique.

当没有合适的值作为key的时候，你最好在模型中加一个id属性，或者hash某个值作为key。这个key只需要在兄弟节点中唯一，并不需要全局唯一。

As a last resort, you can pass item's index in the array as a key. This can work well if the items are never reordered, but reorders will be slow.

还有一个办法，你可以使用数组的序号作为key。如果你的元素没有改变顺序的需求的话，就没有问题，但是如果有改变顺序的功能就会非常慢。

# Tradeoffs 设计的折中

It is important to remember that the reconciliation algorithm is an implementation detail. React could rerender the whole app on every action; the end result would be the same. We are regularly refining the heuristics in order to make common use cases faster.

目前，比对算法仍在改进中。react有可能会在每个动作之后都重新渲染整个app；并且渲染的结果可能都是相同的。我们正在不停的改进算法保证大部分的场景都是最高效的。

In the current implementation, you can express the fact that a subtree has been moved amongst its siblings, but you cannot tell that it has moved somewhere else. The algorithm will rerender that full subtree.

在目前的实现中，如果在兄弟节点中移动一个节点，是没有问题的，如果将这个节点移动到其它的地方去，会导致重新构建这个节点。

Because React relies on heuristics, if the assumptions behind them are not met, performance will suffer.

1. The algorithm will not try to match subtrees of different component types. If you see yourself alternating between two component types with very similar output, you may want to make it the same type. In practice, we haven't found this to be an issue.
2. Keys should be stable, predictable, and unique. Unstable keys (like those produced by Math.random()) will cause many component instances and DOM nodes to be unnecessarily recreated, which can cause performance degradation and lost state in child components.

如果上面提到的2个条件不能被满足，性能会比较差。

1. 目前的算法不会去比对类型不同节点的孩子节点。如果你的两个节点有着很类似的输出，最好保证其有相同的类型。在实际的应用中，这一般都不是一个问题。 

2. keys应该是稳定不变的，可预见的，并且唯一的。不稳定的key（比如用Math.random()产生的）将会导致大量的重复构建，这将会导致性能下降，也会导致孩子节点的state状态丢失。

