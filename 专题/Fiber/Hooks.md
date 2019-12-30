只有当`Fiber`对象更新后，才会更新到`ClassComponent`上的`this.state`和`this.props`上

**`this`上的`state`和`props`是根据`Fiber`对象的`state`、`props`更新的。**

这实际上也方便了`ReactHooks`，因为`hooks`是为`FunctionalComponent`服务的。虽然`FunctionalComponent`没有`this`，但`Fiber`上有，是可以拿到`state`和`props`的


作者：进击的小进进链接：https://juejin.im/post/5d5aa4695188257573635a0d来源：掘金著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

