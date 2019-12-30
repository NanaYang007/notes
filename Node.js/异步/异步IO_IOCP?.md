##### 请求对象

在uv_fs_open()的调用过程中，我们创建一个FSReqWrap请求对象，从js层传入的参数和当前方法都被封装在这个请求对象中，回调函数被设置在这个对象的oncomplete_sym属性上。

请求对象被推入线程池中等待执行。



#### 🤔️IOCP

事􏰹􏰺环、􏰻􏰼者、请􏰃对􏰄、I/O线程􏰎这􏱏者􏱐􏱑􏱒􏰢了Node􏰁􏰂I/O􏱓􏱔的􏱕本要􏱖。 



**setImmediate / process.nextTick**

![image-20191113124535242](/Users/test/Library/Application Support/typora-user-images/image-20191113124535242.png)

![image-20191113124709146](/Users/test/Library/Application Support/typora-user-images/image-20191113124709146.png)



**事件驱动与高性能服务器**

事件驱动：主循环加事件触发的方式运行程序。

知名服务器Nginx，也摒弃了多线程的方式，采用了和Node相同的事件驱动，大有取代Apache之势。