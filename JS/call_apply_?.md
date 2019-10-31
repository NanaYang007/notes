##### call / apply区别

它们作用一样，区别在于传入参数

- apply接受两个参数，第一个参数指定了函数体内this对象的指向，第二个参数为一个带下标的集合（数组、类数组）； - 使用频率高
- call传入的参数数量不固定，跟apply相同，第一个参数指定了函数体内this对象的指向，从第二个参数开始往后，依次传入函数。



##### 传入null时

如果第一个参数是null，在浏览器里this指向window，use strict后指向null。

```js
function func(){
  console.log(this === window);  // true
}
func.apply(null, [1,2,3])
```

⚠️使用use strict直接调用是undefined（见this.md），通过apply调用是null。

**为什么要传入null？**

有时候使用call/apply不是为了改变this指向，而是用于调用其他对象的方法

```js
// 对比
Math.max(1,2,3,4,5);  // 5
Math.max.apply(null, [1,2,3,4,5]);  // 5
```



#### 用途

**1# 改变this指向**

```js
document.getElementById('div1').onclick = function(){
  var func = function(){
    alert(this.id);
  }
  func.call(this);
}
```



**2#  ⚠️Function.prototype.bind**

bind也是改变函数指向，不同的是，func.bind(obj)后，函数改变了指向，但是并没有执行，可以在用户需要的时候再调用func

**手写bind** 

```js
Function.prototype.bind = function( context ){
 var self = this; // 保存原函数
 return function(){ // 返回一个新的函数
 return self.apply( context, arguments ); // 执行新的函数的时候，会把之前传入的 context
 // 当作新函数体内的 this
 }
};
var obj = {
 name: 'sven'
};
var func = function(){
 alert ( this.name ); // 输出：sven
}.bind( obj);
func(); 
```

**？？return/return**🤔️

复杂版：

```js
Function.prototype.bind = function(){
 var self = this, // 保存原函数
 context = [].shift.call( arguments ), // 需要绑定的 this 上下文
 args = [].slice.call( arguments ); // 剩余的参数转成数组
 return function(){ // 返回一个新的函数
 return self.apply( context, [].concat.call( args, [].slice.call( arguments ) ) );
 // 执行新的函数的时候，会把之前传入的 context 当作新函数体内的 this
 // 并且组合两次分别传入的参数，作为新函数的参数
 }
 };
var obj = {
 name: 'sven'
};
var func = function( a, b, c, d ){
 alert ( this.name ); // 输出：sven
 alert ( [ a, b, c, d ] ) // 输出：[ 1, 2, 3, 4 ]
}.bind( obj, 1, 2 );
func( 3, 4 ); 
```



**3#  借用其他对象的方法**

> 杜鹃不会筑巢，也不会孵雏，而是把自己的蛋寄托给云雀等其他鸟类，让它们孵化和养育。

3-1 借用构造函数（省略）

3-2 类数组借用数组方法

arguments有下标，但是并非真正的数组，没有push方法

```js
(function(){
  Array.prototype.push.call(arguments, 3);
  console.log(arguments);  // [1,2,3]
})(1,2)
```

push的内部实现是属性复制+改变length

```js
function ArrayPush() {
 var n = TO_UINT32( this.length ); // 被 push 的对象的 length
 var m = %_ArgumentsLength(); // push 的参数个数
 for (var i = 0; i < m; i++) {
 this[ i + n ] = %_Arguments( i ); // 复制元素 (1)
 }
 this.length = n + m; // 修正 length 属性的值 (2)
 return this.length;
}; 
```

可以把“任意”对象传入Array.prototype.push

```js
var a = {};
Array.prototype.push.call(a, 'first');
alert(a.length);  // 1
alert(a[0]);   // first
```



疑问：手写bind

























