影响发送窗口的两个限制：接收窗口大小、网络的拥塞点（能导致网络拥塞的数据量）。拥塞的结果就是丢包，是发送方最忌惮的。



维护拥塞窗口：

慢启动（2+2, 4+4, 8+8）-> 临近窗口值（参考过去的拥塞点、接口窗口值）+1（16+1, 17+1）

![image-20191012154055223](/Users/test/Library/Application Support/typora-user-images/image-20191012154055223.png)



发生拥塞后：

![image-20191012155754131](/Users/test/Library/Application Support/typora-user-images/image-20191012155754131.png)



超时重传是很严重的，RTO造成网络瓶颈。所以当发送方收到三个重复ACK时，会重传对应Seq的包，称为<b>快速重传</b>，它不用像RTO一样等待一段时间。而设置为三个是因为可能有乱序到达的情况，但是乱序距离一般不会相差太大，比如2号包跑到4号包后面，但是不太可能跑到6号包后面，所以限定成3个或以上在很大程度上避免了因乱序而触发快速重传。



发生快速重传后：

![image-20191012160837089](/Users/test/Library/Application Support/typora-user-images/image-20191012160837089.png)



SACK

![image-20191012161054639](/Users/test/Library/Application Support/typora-user-images/image-20191012161054639.png)



#### TCP算法

Westwood算法与Vegas算法

RFC2001把临界窗口值定义为发生丢包时拥塞窗口的一半大小。

Westwood算法先推算出有多少包已经被送达接收方（根据接收方回应的Ack来推算），当丢包很轻微时，由于Westwood能估算出当时拥塞并不严重，所以不会大幅度减小临界窗口值，传输速度也能得以保持。在经常发生非拥塞性丢包的环境中（比如无线网络），Westwood最能体现优势。

![image-20191012184109907](/Users/test/Library/Application Support/typora-user-images/image-20191012184109907.png)



Vegas引入全新理念，通过监控网络状态来调整发包速度，实现真正的“拥塞避免”。当网络状况良好时，数据包的RTT（往返时间）比较稳定，这时候就可以增大拥塞窗口；当网络开始繁忙时，数据包开始排队，RTT就会变大，这时候就需要减少拥塞窗口了。该设计最大优势在于，在拥塞真正发生之前，发送方已经能通过RTT预测到，并且通过减慢发送速度来避免丢包的发生。



后端给的图：

![e4f34327f58c8a44548c5688a436c5c9](/Users/test/Library/Application Support/qunarChat/downloads/e4f34327f58c8a44548c5688a436c5c9.png)



![6f3ac1c1912073e313e518bfd1d1a96e](/Users/test/Library/Application Support/qunarChat/downloads/6f3ac1c1912073e313e518bfd1d1a96e.png)