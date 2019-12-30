DNS同时支持TCP和UDP，通过DNS看两者区别。我们向DNS服务器发起DNS查询：

![image-20191012110406476](/Users/test/Library/Application Support/typora-user-images/image-20191012110406476.png)

DNS由于连接消耗较大，所以默认采用UDP，可以设置使用TCP：

set vc

![image-20191012112622980](/Users/test/Library/Application Support/typora-user-images/image-20191012112622980.png)

抓包：DNS使用TCP

![image-20191012112511254](/Users/test/Library/Application Support/typora-user-images/image-20191012112511254.png)

