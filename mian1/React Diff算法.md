## 动机

当你使用 React ，在任何一个单点时刻你可以认为 `render()` 函数的作用是创建 React 元素树。在下一个 state 或props 更新时，`render()` 函数将会返回一个不同的 React 元素树。接下来 React 将会找出如何高效地更新 UI 来匹配最近时刻的 React 元素树。

目前存在大量通用的方法能够以最少的操作步骤将一个树转化成另外一棵树。然而，这个[算法是复杂度](http://grfia.dlsi.ua.es/ml/algorithms/references/editsurvey_bille.pdf)为O(n3)，其中n 为树中元素的个数。

如果你在 React 中展示 1000 个元素，那么每次更新都需要10亿次的比较，这样的代价过于昂贵。然而，React 基于以下两个假设实现了时间复杂度为 O(n) 的算法:

1. **不同类型的两个元素将会产生不同的树。**
2. **开发人员可以使用一个 `key` prop 来指示在不同的渲染中那个那些元素可以保持稳定。**

事实上，这些假设在几乎所有的用例中都是有效的。



## Diffing 算法

当比较不同的两个树，React 首先比较两个根元素。根据根跟的类型不同，它有不同的行为。

### 元素类型不相同

无论什么时候，当根元素类型不同时，React 将会销毁原先的树并重写构建新的树。从 `<a>` 到 `<img>` ，或者从 `<Article>` 到 `<Comment>` ，从 `<Button>` 到 `<div>` – 这些都将导致全部重新构建。

当销毁原先的树时，之前的 DOM 节点将销毁。实例组件执行 `componentWillUnmount()` 。当构建新的一个树，新的 DOM 节点将会插入 DOM 中。组件将会执行 `componentWillMount()` 以及 `componentDidMount()` 。与之前旧的树相关的 state 都会丢失。

根节点以下的任何组件都会被卸载(unmounted)，其 state(状态)都会丢失。例如，当比较:

```
<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```

这会销毁旧的 `Counter` ，并重新装载（remount）一个新的。

### DOM元素类型相同

当比较两个相同类型的 React DOM 元素时，React 检查它们的属性（attributes），保留相同的底层 DOM 节点，只更新反生改变的属性（attributes）。例如：

```
<div className="before" title="stuff" />

<div className="after" title="stuff" />
```

通过比较两个元素，React 会仅修改底层 DOM 节点的 `className` 属性。

当更新 `style`属性，React 也会仅仅只更新已经改变的属性，例如:

```
<div style={{'{{'}}color: 'red', fontWeight: 'bold'}} />

<div style={{'{{'}}color: 'green', fontWeight: 'bold'}} />
```

当React对两个元素进行转化的时候，仅会修改`color`，而不会修改 `fontWeight` 。

在处理完当前 DOM 节点后，React 会递归处理子节点。

### 相同类型的组件

当一个组件更新的时候，组件实例保持不变，以便在渲染中保持state。React会更新组件实例的属性来匹配新的元素，并在元素实例上调用 `componentWillReceiveProps()` 和 `componentWillUpdate()`。

接下来， `render()` 方法会被调用并且diff算法对上一次的结果和新的结果进行递归。

### 子元素递归

默认情况下，当递归一个 DOM 节点的子节点时，React 只需同时遍历所有的孩子基点同时生成一个改变当它们不同时。

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

### Keys

为了解决这个问题，React 支持一个 `key` 属性（attributes）。当子节点有了 key ，React 使用这个 key 去比较原来的树的子节点和之后树的子节点。例如，添加一个 `key` 到我们上面那个低效的例子中可以使树的转换变高效：

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

现在 React 知道有`'2014'` key 的元素是新的， key为`'2015'` 和`'2016'`的两个元素仅仅只是被移动而已。

实际上，找到一个 key 通常不难。你所将要展示的组件一般都有唯一的ID，因此你的数据可以作为key的来源：

```
<li key={item.id}>{item.name}</li>
```

当情况不同时，你可以添加一个新的ID 属性（property）到你的数据模型，或者是hash 一部分内容生成一个key。这个key 需要在它的兄弟节点中是唯一的就可以了，不需要是全局唯一。

作为最后的手段，你可以将数组中的索引作为 key 。如果它们从不重新排序，它们工作也很好，但是如果存在重新排序，性能将会很差。

## 权衡利弊

需要记住的是 reconciliation 算法(reconciliation algorithm)仅仅只是一个实现细节。React 会在每个操作上重新渲染整个应用，最终的结果可能是相同的。我们经常细化启发式算法，以便优化性能。

在当前实现中，你可以表达这样一个事实，子树已经在兄弟节点中被移除，但是你不必告诉被移到什么位置。这个算法将会重新渲染整个子树。

因为React 依赖这个启发式，如果它们背后的假设没有得到满足，性能将会受到影响。

1. 算法不会尝试匹配不同节点类型的子树。如果你发现在有类似输出的两个不同节点类型中相互切换，你可能需要将其转化成同种类型，事实上，我们没有在其中发现问题。
2. keys 应该是稳定的、可预测的并且是唯一的。不稳定的 key (类似于 `Math.random()` 函数的结果)可能会产生非常多的组件实例并且 DOM 节点也会非必要性的重新创建。这将会造成极大的性能损失和组件内state的丢失。