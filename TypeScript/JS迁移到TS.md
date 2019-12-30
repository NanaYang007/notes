**自己编写的文件**

.js变成.ts不会有编译变化，ts中的js代码在编译后与原始的js代码一模一样。

使用any减少错误



**第三方代码**

创建vender.d.ts文件作为开始（.d.ts文件拓展名指定这个文件是一个声明文件）

https://github.com/DefinitelyTyped/DefinitelyTyped



**使用@types**

```text
npm install @types/jquery --save-dev
```



**类型断言**

```tsx
interface Foo {
  bar: number;
  bas: string;
}
const foo as Foo;
for.bar = 123;
foo.bas = 'hello';
```

双重断言：

![image-20191112124633753](/Users/test/Library/Application Support/typora-user-images/image-20191112124633753.png)

