#### 流程

excel导入一个5千行左右的gk列表，以json格式POST方法传给后台，报413错

#### 抓包

🤔️ 服务器端发出Payload too large的时候 Ack是28kb，koa的默认限制是1mb，egg的限制是100kb，总共传输133kb。28kb是为什么呢？

![image-20191015101238829](/Users/test/Library/Application Support/typora-user-images/image-20191015101238829.png)



![image-20191015101248759](/Users/test/Library/Application Support/typora-user-images/image-20191015101248759.png)

