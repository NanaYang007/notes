Its headline feature is **incremental rendering**: the ability to split rendering work into chunks and spread it out over multiple frames.



### Review

Reconciliation  -  virtual DOM.  diff算法

update - setState的结果

> The reconciler does the work of computing which parts of a tree have changed; the renderer then uses that information to actually update the rendered app.

> This separation means that React DOM and React Native can use their own renderers while sharing the same reconciler, provided by React core.

scheduling

the process of determining when work should be performed.

work

any computations that must be performed. Work is usually the result of an update (e.g. setState).

#### 关键点

不用每一个变化都及时响应

用户输入优先级高于网络数据展示更新

push / pull

