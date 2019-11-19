##### 问题：

数据库里看的是2004-05-03 17:30:08

我在我们的系统里select出来就是2004-05-03T09:30:08



##### 发现问题：

ISO8601标准时间格式里，一个时间比如北京下午17:30，标准写法是2004-05-03T17:30:08+08:00



#####数据库配置：

show variables like 'time_zone'

我的mysql是 +8.00



#####问题定位：

数据库连接没有设置好时区（jdbc也有时区配置）

https://blog.csdn.net/qq631431929/article/details/51731834

Egg-mysql没有调整时区的配置，egg-sequelize有

https://www.jianshu.com/p/99dc4d9e5713

![image-20191118202335575](/Users/test/Library/Application Support/typora-user-images/image-20191118202335575.png)

![image-20191119152022472](/Users/test/Library/Application Support/typora-user-images/image-20191119152022472.png)

#####后记：

mysql的timestamp用法详解：

https://www.cnblogs.com/givemelove/p/8251185.html

js可以通过：

new Date().getTimezoneOffset()

得到以分钟计的与GMT的差，北京是-480



#####资料：

UTC与ISO-8601

https://blog.csdn.net/u014788227/article/details/88306507

16-01-18T23:41:00 是符合 ISO-8601 标准的时间表示。

2016-01-18T23:41:00 里面的 T 表示 UTC，所以这个字符串解析后就表示 UTC 时间的 2016-01-18 23:41:00，那么再转换为北京当地时间展示（比如，在 JavaScript 里面 new Date('2016-01-18T23:41:00').toLocaleString()）时就会加上 8 小时的偏移，变成：2016-01-19 7:41:00



#####测试：

![image-20191112185121036](/Users/test/Library/Application Support/typora-user-images/image-20191112185121036.png)

你在js中出现GMT（民间的UTC）

https://zhidao.baidu.com/question/153332934.html



一些js控制台测试：

![image-20191112185536260](/Users/test/Library/Application Support/typora-user-images/image-20191112185536260.png)

