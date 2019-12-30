⚠️Buffer占用的内存不是通过V8分配的，属于堆外内存。

```node
var str = '深入浅出node.js';
var buf = new Buffer(str, 'utf-8');
console.log(buf); //<Buffer e6 b7 b1 e5 85 a5 e6 b5 85 e5 87 ba 6e 6f 64 65 2e 6a 73>
```

new Buffer(100)；buffer[10]这样获取buffer的每一个元素都是0-255的整数。



#### 分配

slab动态内存管理机制，Node以8kb为界限来区分Buffer是大对象还是小对象

![image-20191119190316413](/Users/test/Library/Application Support/typora-user-images/image-20191119190316413.png)

Buffer对象都是JS层面的，能够被v8的垃圾回收标记回收，但是其内部的parant属性指向的SlowBuffer对象来自于Node自身C++中的定义，是C++层面上的Buffer对象，所用内存不在v8的堆中。



#### 转换

- 字符串转成Buffer

- ```node
  new Buffer(str, [encoding]);
  //存储多种编码类型
  buf.write(string, [offset], [length], [encoding]);
  ```

- Buffer转成字符串

- ```node
  buf.toString([encoding], [start], [end]);
  ```



#### 拼接

宽字节字符串被截断问题（如中文）

decoder避免乱码：

![image-20191119191330085](/Users/test/Library/Application Support/typora-user-images/image-20191119191330085.png)

⚠️读取一个相同的大文件􏰧􏰨时，**highWaterMark**值􏰈的大小与读取速度􏶰的关系：该值越大，读取速度越快􏶯􏶰􏳊􏶲。 

