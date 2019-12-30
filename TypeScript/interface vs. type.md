##### 定义

```tsx
interface Point {
  readonly x: number;
}

type Point = {
  readonly x: number;
}
```

interface和type都能描述对象/函数。

type还可以用于其他类型，如基本类型（原始值）、联合类型、元组。

```tsx
type Name = string;
// union
type PartialPointX = { x: number };
type PartialPointY = { y: number };
type PartialPoint = PartialPointX | PartialPointY;
// typle
type Data = [number, string];
// dom
let div = docuemnt.createElement('div');
type B = typeof div;
```



##### 拓展 Extend

```tsx
interface PartialPointX { x: number };
interface Point extends PartialPointX { y: number};

type PartialPointX = { x: number };
type Point = PartialPointX & { y: number };
```



(to be continued...)

https://www.cnblogs.com/EnSnail/p/11233592.html