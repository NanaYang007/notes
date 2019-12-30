Diff 就是新旧节点的对比，在上一篇（自顶向下.md）中也说道了，这里面的 Diff 主要是构建 currentInWorkProgress 的过程，同时得到 Effect List，给下一个阶段 commit 做准备。



##### Diff算法从reconcileChildren开始

```js
function reconcileChildren(current$$1, workInProgress, nextChildren, renderExpirationTime) {
  if (current$$1 === null) {
    workInProgress.child = mountChildFibers(workInProgress, null, nextChildren, renderExpirationTime);
  } else {
    workInProgress.child = reconcileChildFibers(workInProgress, current$$1.child, nextChildren, renderExpirationTime);
  }
}
```

首次渲染，`current$$1`为空，通过`mountChildFibers`创建子节点的Fiber实例；不是首次渲染，调用`reconcileChildFibers`去做diff，然后得出`Effect List`。



https://mp.weixin.qq.com/s/lWyqHfHFAstS6AhfaHe7Iw

