#### The Diffing Algorithm

* 如果两个root elements有不一样的type，React会拆卸旧的树并且构建新的树
* 同样的type，只会更新不一样的props，比如color等等
* 在有key的情况下，效率会高（没有key时，如果一个list后面加一条，会只insert加的那一条；如果list第一条前面加一条，会整个更新。如果有key，就只会更新新的key那一条）



React官网上说：Visual DOM的过程就是reconciliation

> The virtual DOM (VDOM) is a programming concept where an ideal, or “virtual”, representation of a UI is kept in memory and synced with the “real” DOM by a library such as ReactDOM. This process is called [reconciliation](https://reactjs.org/docs/reconciliation.html).
>
> https://reactjs.org/docs/faq-internals.html#what-is-react-fiber

（前置知识： notes / React / Element.md）