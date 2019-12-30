(接着react-fiber-architecture)

### Fiber

* pause
* assign
* reuse
* abord



##### First, break down

A fiber  —— a unit of work.  As a virtual stack frame.

Customize the behavior of the call stack to optimize for rendering UIs.



##### Structure of a fiber

* Type key
* Child sibling

![image-20191126164127777](/Users/test/Library/Application Support/typora-user-images/image-20191126164127777.png)

* return
* pendingProps memorizedProps ???  —— reuse

![image-20191126164449720](/Users/test/Library/Application Support/typora-user-images/image-20191126164449720.png)

* pendingWorkPriority

https://github.com/facebook/react/blob/master/src/renderers/shared/fiber/ReactPriorityLevel.js



![img](https://user-gold-cdn.xitu.io/2019/9/16/16d3aae256e48d4c?imageslim)