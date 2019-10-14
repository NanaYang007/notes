#### 最大传输单元(Maximum Transmission Unit, MTU)

数据链路层对包大小的限制称为【最大传输单元】，以太网的帧最小为64字节，除去14字节头部和4字节CRC字段，有效荷载为46字节；最大帧为1518字节，除去...有效荷载最大为1500字节，即以太网的MTU。如果传输100kb的数据，至少需要（100*1024/1500=）69个以太网帧。

通过netstat -i命令可以查看MTU

抓包中IP协议中：

Flags. More fragments: Set，Fragment offset: 185（wireshark这里显示的时候除以了8，真实的分片偏移量为185*8=1480）表示还有后续包。



#### TCP最大段大小(Max Segment Size, MSS)

TCP为了避免被发送方分片，会主动把数据分割成小段再交给网络层，最大的分段大小称之为MSS（Max Segment Size）



<span style="background-color:#DDD;padding:20px;margin-top:10px">MSS = MTU - IP header头大小 - TCP头大小</span>



#### 总结

"IP 数据包长度在超过链路的 MTU 时在发送之前需要分片，而 TCP 层为了 IP 层不用分片主动将包切割成 MSS 大小。在握手时双方会传递自己的MSS，以小的一方作为发送时的MTU。"