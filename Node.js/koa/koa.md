原生Node.js 开启http服务

```js
const http = require('http');
http.createServer((req,res)=>{ 
  res.writeHead(200); 
  res.end('hi koala');
}).listen(3000);
```



koa2开启http服务

```js
const Koa = require('koa');
const app = new Koa();
const {createReadStream} = require('fs');
app.use(async (ctx,next)=>{ 
  if(ctx.path === '/favicon.ico'){ 
    ctx.body = createReadStream('./avicon.ico')
  }else{ 
    await next(); 
  }
});
app.use(ctx=>{ ctx.body = 'hi koala';})
app.listen(3000);
```

* 模块化支持很好；
* req，res都封装到ctx中；
* **中间件机制**，采用洋葱模型，流程：洋葱是先从皮到心，然后从心到皮。通过洋葱模型把代码流程化，让流水线更加清楚，如果不使用中间件，在createServer一条线判断所有逻辑确实不好。

