域名有顶级域名（baidu.com），二级域名（www.baidu.com）之分，多个域名指向一个网站服务器上时，需要将这些子域名（顶级域名下面的二级域名、三级域名都称之为子域名）设置并指向自己的网站服务器上的，这个动作一般称之为A记录，又称IP指向。

A记录 —— 一个域名解析到一个IP

比如www.baidu.com（二级域名）解析到8.8.8.8



说到这里实际上就会产生一个问题，就是当服务器需要更换时，这些原本指向这台服务器的域名就需要重新设置，并指向新的服务器，这样就会产生比较大的工作量。

CNAME（别名记录） —— 多个名字映射到另一个域名

比如 a.www.baidu.com 解析到www.baidu.com



总结：

CNAME提供了单一服务器和海量服务器的在管理访问上的灵活性。单一服务器的场景下，通过将大量子域名指向到CNAME，再由 CNAME 指向到单一域名，解决了服务器更换、迁移带来的大量域名重新指向的问题。

另一方面，CNAME配合负载均衡系统，还可以实现将大量访问需求通过CNAME指向到多台服务器，以提高用户访问的速度。



作者：诺曼底的救赎
链接：https://www.jianshu.com/p/65757b5c0762