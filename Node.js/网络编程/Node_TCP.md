TCP服务在网络应用中十分常见，目前大多数的应用都是基于TCP搭建而成的。

___

例子：

##### server

```js
var net = require('net');
var server = net.createServer(function(socket){
  socket.on('data', function(data){
    console.log(data.toString());
    socket.write('你好');
  });
  socket.on('end', function(){
    console.log('连接断开');
  });
  socket.write('欢迎光临《深入浅出Node.js》');
});

server.listen(8124, function(){
  console.log('server bound');
})
```

##### client

```js
var net = require('net');
var client = net.connect({port: 8124}, function(){
  console.log('client connect');
  client.write('world!\r\n');
})
client.on('data', function(data){
  console.log(data.toString());
  client.end();
})
client.on('end', function(){
  console.log('client disconnect');
})
```



##### 简单聊天室

##### server

```js
var net = require('net');
process.stdin.setEncoding('utf8');
var server = net.createServer(function(socket){
  process.stdin.on('readable', () => {
    let chunk = process.stdin.read();
      if(chunk){
        socket.write(chunk.toString());
      }
    }
  );
  socket.on('data', function(data){
    data = data.toString().replace(/\r|\n/, '');
    if(data !== 'exit'){
      console.log('小客: ' + data.toString());
    } else {
      socket.end();
      console.log('服务器已断开连接');
    }
  })
});
server.listen(8124, function(){
  console.log('等待用户输入: (输入exit退出)');
})
```



##### client

```js
var net = require('net');
var client = net.connect({port: 8124}, function(){
  console.log('等待用户输入: (输入exit退出)');
});
client.on('data', function(data){
  data = data.toString().replace(/\r|\n/, '');
  if(data === 'exit'){
    client.end();
    console.log('小客已断开连接');
  } else {
    console.log('小服: ' + data, '');
  }
});
process.stdin.on('readable', function(){
  let chunk = process.stdin.read();
  if(chunk){
    client.write(chunk.toString());
  }
});
```

