**没有sleep函数**

⚠️不能使用new Date()，然后while循环判断 —— 持续占用CPU进行判断，与真正的线程沉睡相去甚远，破坏了时间循环的调度。

正确做法是整理好业务逻辑以后，使用setTimeout。



###**# 发布/订阅模式**

![image-20191113151701325](/Users/test/Library/Application Support/typora-user-images/image-20191113151701325.png)

为了处理异常，EventEmitter对象对error事件进行了特殊对待。如果运行期间的错误触发了error事件，EventEmitter会检查是否有对error事件添加侦听器。如果添加了，错误会交它处理，否则这个错误将会作为异常抛出。**如果外部没有捕获这个异常，将会引起线程退出。**一个健壮的EventEmmiter实例应该对error事件做处理。



**once**

![image-20191113160702189](/Users/test/Library/Application Support/typora-user-images/image-20191113160702189.png)



**哨兵变量**

事件通常是一对多，也存在多对一、多对多的场景：

![image-20191114141951377](/Users/test/Library/Application Support/typora-user-images/image-20191114141951377.png)



### # Promise / Deferred模式

“使用事件的方式时，执行流程需要预先设定。即便是分支，也需要预先设定，这是由发布/订阅模式的运行机制决定的。是否有一种**先执行异步调用，延迟传递处理的方式？**” 答案是Promise / Deferred模式。



“Promise/Deferred模式在2009年作为一个提议草案，发布在C o m mo n JS规范中，随着使用Promise/Deferred模式应用逐渐增多，草案抽象出了Promises/A、Promises/B、Promises/D这样典型的异步P/D模型，使得异步操作可以以一种优雅的方式出现。“



**⚠️ Promises/A**

(Promise的几条性质)

<span style="color:#46aa22;font-weight:bold">为什么</span>使用Promise？

对于嵌套很深的异步操作，传统写法造成“嵌套地狱”，如果采用事件形式，会造成代码增多，所以使用**链式调用**(then().then())。

