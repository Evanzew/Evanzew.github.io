---
layout: post
title: React Reconciliation(调和)
date: 2018-11-26 11:27:24.000000000 +09:00
---

#### 动机

当使用 React 时，在下一个 steate 或 props 更新时，render()函数将会返回一个不同的 React 树，接下来 React 会找出最高效地方式更新 UI。
目前存在大量通用的方法能够以最少的操作步骤将一个树转化成另外一棵树。然而，这个算法是复杂度为 O(n<sup>3</sup>)，其中 n 为树中元素的个数。即如果有 1000 个元素，那么每次更新都要进行 10 亿次比较。
然而，React 给予一下两个假设实现了复杂度为 O(n)的算法，即每次更新只需要进行 1000 次的比较：

1. 不同类型的两个元素将会产生不同的树。
2. 开发人员可以使用一个 key prop 来指示在不同的渲染中那个那些元素可以保持稳定。

### 一. Diffing 算法

当比较不同的两个树的时候，React 首先比较两个根元素。根据很元素的类型，他有不同的行为。

##### 1.1 元素类型不相同

无论什么时候，当根元素类型不同时，React 将会销毁原先的树并重新构建新的树。
当销毁原先的树时，之前的 DOM 节点将全部被销毁。组件会执行`componentWillUnmount()`。当构建新的树时，新 DOM 节点将会插入 DOM 中。组件将会执行`componentWillMount()` 以及 `componentDidMount()` 。与之前旧的树相关的 state 都会丢失。

##### 1.2 DOM 元素类型相同

当比较两个相同类型的 React DOM 元素时，React 检查它们的属性（attributes），保留相同的底层 DOM 节点，只更新反生改变的属性（attributes）。
当处理完当前 DOM 节点后，React 会递归处理子节点。

##### 1.3 相同类型的组件

当一个组件更新的时候，组件实例保持不变，以便在渲染中保持 state。React 会更新组件实例的属性来匹配新的元素，并在元素实例上调用 `componentWillReceiveProps()` 和 `componentWillUpdate()`。

##### 1.4 子元素递归

例如，当给子元素末尾添加一个元素，在两棵树之间转化中性能就不错:

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

React 会比较两个 `<li>first</li>` 树与两个 `<li>second</li>` 树，然后插入 `<li>third</li>` 树。

如果在开始处插入一个节点也是这样简单地实现，那么性能将会很差。例如，在下面两棵树的转化中性能就不佳。

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

React 将会改变每一个子节点而没有意识到需要保留 `<li>Duke</li>` 和 `<li>Villanova</li>` 两个子树。这种低效是一个问题。

##### 1.5 Keys

React 提供了一个 key 属性(attributes)。当节点有了 key，React 使用 key 去比较原来的书的子节点和之后的节点。当我们添加一个 key 时，可以是上面抵消的例子转换变高效:

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

此时 React 知道只有`key="2014"`的元素是新的，剩下的两个元素仅仅只是被移动而已。这样就会保留剩下的两个元素只更新新加入的元素。

##### 1.6 总结

因为 React 依赖这两个启发式，如果背后的假设没有得到满足，性能将会受到影响。

1.  算法不会尝试匹配不同节点类型的子树。
2.  keys 应该是稳定的，可预测的并且是唯一的。不稳定的 key (类似于 Math.random() 函数的结果)可能会产生非常多的组件实例并且 DOM 节点也会非必要性的重新创建。这将会造成极大的性能损失和组件内 state 的丢失
