#### 四个this指向（清晰）

##### 1#  function作为对象的方法调用

指向对象

```js
var obj = {
  a: 1,
  getA: function(){
    alert(this === obj);  //true
    alert(this.a);   // 1
  }
};
obj.getA();
```



##### 2#  function作为普通函数调用

指向全局变量，浏览器里是window对象

```js
window.name = 'globalName';
var myObject = {
  name: 'sven',
  getName: function(){
    return this.name;
  }
};
var getName = myObject.getName;
console.log(getName());  // globalName
```

(丢失this)

在div内的callback中，作为普通函数调用会丢失div节点，通过保存div节点引用：

```js
document.getElementById('div1').onclick = function(){
  alert(this.id);   // div1
  var callback = function(){
    alert(this.id);   // window
  }
  callback();
}

// 解决
document.getElementById('div1').onclick = function(){
  var that = this;
  var callback = function(){
    alert(that.id);   // window
  }
  callback();
}
```

在ECMAScript5的strict模式下，这种情况下的this已经被规定不会指向全局变量，而是undefined：

```js
function func(){
  'use strict'
  alert(this);   // undefined
}
func();
```



##### 3#  function作为构造器调用

指向创建的对象

⚠️如果构造器显式返回一个**对象**(字符串等依旧返回this)，则最终返回那个对象而不是this

```js
var MyClass = function(){
  this.name = 'sven';
  return {  // 显式返回一个对象
    name: 'anne'
  }
}
var obj = new MyClass();
alert(obj.name);  // anne
```



##### 4#  Function.prototype.call / Function.prototype.apply调用

call/apply可以动态地改变传入函数的this

```js
var obj1 = {
  name: 'sven',
  getName: function(){
    return this.name;
  }
};
var obj2 = {
  name: 'anne'
};
console.log(obj1.getName.call(obj2));  // anne
```

疑问🤔️：

call 和 apply 方法能很好地体现 JavaScript 的函数式语言特性，在 JavaScript 中，几乎每一次 编写函数式语言风格的代码，都离不开 call 和 apply。在 JavaScript 诸多版本的设计模式中，也 用到了 call 和 apply。



####实际使用

##### 丢失this

如上面的function作为普通函数调用的代码，会发生丢失this。当我们想用短的名字替代document.getElementById时

```js
// 可使用
var getId = function(id){
  return document.getElementById(id)
};

// 不可使用
var getId = document.getElementById;
```

因为getElementById中使用了this，当它作为对象方法调用时，this指向document，当使用第二种方法，this丢失。

我们尝试利用apply把document当作this传入getId函数，帮忙修正this：

```js
document.getElementById = (function(func){
  return function(){
    return func.apply(document, arguments);
  }
})(document.getElementById);
var getId = document.getElementById;
alert(getId('div1').id);  // div1
```































