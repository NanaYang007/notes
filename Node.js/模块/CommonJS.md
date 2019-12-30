#### CommonJS

CommonJS 规范为 JS 制定了一个美好的愿景 —— 希望JS能够在任何地方运行。规范涵盖了模块、二进制、Buffer、字符集编码、I/O流、进城环境、文件系统、套接字、单元测试、Web服务器网关接口、包管理等。

**1# 模块引用**

```js
var math = require('math')
```

**2# 模块定义**

exports对象用于导出当前模块的方法或者变量，exports是module的属性。

```js
// file1
exports.add = function(){...}
//file2
var math = require('math');
exports.increment = function(){return math.add();}
```

![image-20191105173105946](/Users/test/Library/Application Support/typora-user-images/image-20191105173105946.png)



##### exports.a赋值 与 module.exports = function赋值

![image-20191105203738612](/Users/test/Library/Application Support/typora-user-images/image-20191105203738612.png)

