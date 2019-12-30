使用 `--noLib` 编译选项会导致 TypeScript 排除自动包含的 `lib.d.ts` 文件

![image-20191112122035248](/Users/test/Library/Application Support/typora-user-images/image-20191112122035248.png)



**String**

全局变量String，StringConstructor接口，String接口

```tsx
interface String{
  endsWith(suffix?: string): boolean
}
String.prototype.endsWith = function(suffix: string): boolean{
  const str: string = this;
  return str && str.indexOf(suffix, str.length-suffix.length)!==-1;
}
console.log('foo bas'.endsWith('bas'));
```

终极String

基于可维护性，我们推荐创建一个 `global.d.ts` 文件。然而，如果你愿意，你可以通过使用 `declare global { /* global namespace */ }`，从文件模块中进入全局命名空间：

```tsx
// 确保是模块
export {};

declare global {
  interface String {
    endsWith(suffix: string): boolean;
  }
}
```

