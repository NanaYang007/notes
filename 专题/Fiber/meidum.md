V16.6.0

一切都是**work**



element种类：

* class component
* function component
* **host** component ( DOM nodes )

element的type是createElement的第一个参数



render用来（is generally used）创建element

##### Fiber

不同type的element要做的改变不同，生成对应type的fiber（描述需要做的事情）

Fiber是代表需要做的工作的数据结构，一个unit of work（可以track，schedule，pause，abord work）

Fiber第一次创建是通过createFiberFromTypeAndProps

之后的改变中，Fiber是重复利用的，并且改变了一些必要的属性。



![image-20191203110712830](/Users/test/Library/Application Support/typora-user-images/image-20191203110712830.png)



第一次渲染完，得到一个Fiber tree，也被称为current。

当React开始work on updates 它构建了一个workInProgress tree（反映未来将会被flush到屏幕上的state）



current上每个Fiber节点的alternate node构成了workInProgress tree。alternate节点的数据来源于render返回的数据。

当workInProgress树render到屏幕上以后，它成为了current tree。



**一致性**是React的一个核心原则，它不会展现部分结果（update in one go）。

每一个Fiber的alternate属性都有一个副本来源于另一棵树，对于workInProgress树上的Fiber也有current的副本。



##### Effect

React中的component就是一个通过state和props计算UI展示的方法，任何其他的活动比如fetch数据、手动改变DOM、调用生命周期方法都作为side-effect，或者简单来说就是effect。

> *We call these operations “side effects” (or “effects” for short) because they can affect other components and **can’t be done during rendering**.*

大多数state和props的update会导致side-effects，应用effect也是一种work，所以Fiber可以关联effect，它们被放入effectTag字段。

Effects list

![image-20191203120536206](/Users/test/Library/Application Support/typora-user-images/image-20191203120536206.png)

React为了非常高效的处理update，引入了一些技术。其中一个是建立一个线性的list（具有effect的Fiber），迭代（遍历）一个线性的list比一棵树要快得多，并且不用在没有side-effect的Fiber节点上花时间。

这个list是**finishedWork**树的子集，由nestEffect属性链接，而不是current / workInProgress树中的child属性。它的起点是**firstEffect**。

![image-20191203120547891](/Users/test/Library/Application Support/typora-user-images/image-20191203120547891.png)

##### fiber root

```js
const fiberRoot = query('#container')._reactRootContainer._internalRoot
const hostRootFiberNode = fiberRoot.current
```

> The fiber tree starts with [a special type](https://github.com/facebook/react/blob/cbbc2b6c4d0d8519145560bd8183ecde55168b12/packages/shared/ReactWorkTags.js#L34) of fiber node which is `HostRoot`. It’s created internally and acts as a parent for your topmost component. There’s a link from the `HostRoot` fiber node back to the `FiberRoot` through the `stateNode` property:

```js
fiberRoot.current.stateNode === fiberRoot; // true
```



> You can explore the fiber tree by accessing the topmost `HostRoot` fiber node through the fiber root. Or you can get an individual fiber node from a component instance like this:

```js
compInstance._reactInternalFiber
```



##### algorithm

React执行工作分为两个阶段：render和commit

？？？

render阶段的work可以是异步的，已完成的work可以被抛弃是基于这个阶段执行的work不会导致任何用户可见的变化，比如DOM更新；而commit阶段永远是同步的，是因为这个阶段的变化直接对用户可见。

> **It’s important to understand that the work during the first** `render` **phase can be performed asynchronously.** React can process one or more fiber nodes depending on the available time, then stop to stash the work done and yield to some event. It then continues from where it left off. Sometimes though, it may need to discard the work done and start from the top again. These pauses are made possible by the fact that the work performed during this phase doesn’t lead to any user-visible changes, like DOM updates. **In contrast, the following** `commit` **phase is always synchronous**. This is because the work performed during this stage leads to changes visible to the user, e.g. DOM updates. That’s why React needs to do them in a single pass.



##### render阶段

reconciliation算法总是从**HostRoot** Fiber节点使用**renderRoot**开始。

但是React会快速的跳过parents直到调用setState方法的component。

work loop

所有Fiber节点都在work loop里被处理。如果是同步，**nextUnitOfWork**引用workInProgress tree中有work要执行的Fiber节点，当遍历完，React离开workloop并开始commit变化。

四个遍历树、初始化work或是完成work的方法：

* performUnitOfWork
* beginWork
* completeUnitOfWork
* completeWork

video动画说明四个函数在遍历树过程中的调用过程：https://vimeo.com/302222454

（我的理解：从一个节点开始走nextUnitOfWork、perform、begin，然后走它的子，如果子有子，则一样，如果没有，则走n、b、c、c，然后走它的c、c，然后走它的sibling，走完走它的父的c、c。nextUnitOfWork的顺序是先子后sibling，最后回到父）。

beginWork返回指向next child的指针。如果有next child，被赋值给nextUnitOfWork；如果没有，则node完成，需要处理它的sibling或是回溯它的parent，这个在completeUnitOfWork中处理。

performUnitOfWork和completeUnitOfWork是为了迭代目的，主要的活动是beginWork和completeWork处理的。

（⚠️ workInProcess是一个Fiber的指针）



##### commit 阶段

从completeRoot开始，React更新DOM并且调用生命周期函数。

effect list是render阶段的产物，整个render阶段就是决定哪个节点更新、删除、插入，以及哪个component需要调用生命周期方法。这就是effect list告诉我们的。

> *For debugging purposes, the* `current` *tree can be accessed through the* `current` *property of the fiber root. The* `finishedWork` *tree can be accessed through the* `alternate` *property of the* `HostFiber` *node in the current tree.*

commit阶段主要方法是commitRoot，它主要做了以下这些事：

* 对于tag为Snapshot的节点调用`getSnapshotBeforeUpdate`；
* 对于tag为Deletion的方法调用`componentWillUnmount`
* DOM插入、更新、删除
* 将finishedWork树set为current
* 对于tag为Placement的节点调用componentDidMount
* 对于tag为Update的节点调用`componentDidUpdate`

> React assigns the `finishedWork` tree to the `FiberRoot` marking the `workInProgress` tree as the `current` tree.

```js
function commitRoot(root, finishedWork) {
    commitBeforeMutationLifecycles()
    commitAllHostEffects();
    root.current = finishedWork;
    commitAllLifeCycles();
}
```

里面每一个function都执行了一个effect list的迭代，当它发现effect属于function的目的就执行。

DOM updates

在commitAllHostEffects方法中执行DOM updates。



https://medium.com/react-in-depth/inside-fiber-in-depth-overview-of-the-new-reconciliation-algorithm-in-react-e1c04700ef6e



第二篇文章

当点击button，ClickCounter组件的Fiber节点具有

```js
{
  stateName: new ClickCounter,
  type: ClickCounter,
  updateQueue: {
  	baseState: { count: 0},
      firstUpdate: {
        next: {
          payload: (state) => {return {count: state.count + 1}}
        }
      }
  }
}
```

###### 1 ClickCounter

每个节点都会调用beginWork方法，在ClickCount执行beginWork中：

找到对应的节点的type的处理case，新建还是update，比如例子中的ClickCounter走的是`updateClassComponent`方法：

* 调用 UNSAFE_componentWillReceiveProps hook
* 处理updates在updateQueue中生成新的state
* 用新的state调用getDerivedStateFromProps
* 调用shouldComponentUpdate，如果是false，跳过整个render阶段
* 调用 UNSAFE_componentWillUpdate
* 增加一个effect去trigger componentDidUpdate方法
* 更新state和props

⚠️在组件render前改变state和props，在结束beginWork后，state和props就是新的了（ `memoizedState` and the `baseState` in `updateQueue`里的baseState，updateQueue里没有update了），并且这时effectTag由0变成3，及Update的side-effect tag。



然后执行`finishClassComponent`，在这里调用`render`方法。



`createWorkInProgress`创建了alternate Fiber nodes，React复制了Fiber中updated的属性。

当ClickCounter组件的子们被reconciled，span的Fiber node将更新pendingProps。



###### 2 button

在`completeUnitOfWork`阶段，button也会被当作nextUnitOfWork，但是不需要对它做什么，所以React move to它的sibling。



###### 3 Span

也是从beginWork开始，现在`nextUnitOfWork`指向span的Fiber的alternate，它属于`HostComponent`。



https://medium.com/react-in-depth/in-depth-explanation-of-state-and-props-update-in-react-51ab94563311



#### React usage of Fiber List

current就是nextUnitOfWork

在workLoop中区分同步 / 异步

```js
function workLoop(isYieldy) {
    if (!isYieldy) {
        // Flush work without yielding
        while (nextUnitOfWork !== null) {
            nextUnitOfWork = performUnitOfWork(nextUnitOfWork);
        }
    } else {
        // Flush asynchronous work until the deadline runs out of time.
        while (nextUnitOfWork !== null && !shouldYield()) {
            nextUnitOfWork = performUnitOfWork(nextUnitOfWork);
        }
    }
}
```

同步的如对用户的交互响应

`shouldYield`被React执行一个Fiber node的work被 **时常** 更新，基于deadlineDidExpire和deadline变量。



https://medium.com/react-in-depth/the-how-and-why-on-reacts-usage-of-linked-list-in-fiber-67f1014d0eb7

