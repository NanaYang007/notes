```ts
function area(s: Shape) {
  switch (s.kind) {
    case 'square':
      return s.size * s.size;
    case 'rectangle':
      return s.width * s.height;
    case 'circle':
      return Math.PI * s.radius ** 2;
    default:
      const _exhaustiveCheck: never = s;
  }
}
```

![0b1c5af2176d942cd33ed0dacfe5c016](/Users/test/Library/Application Support/qunarChat/downloads/0b1c5af2176d942cd33ed0dacfe5c016.jpg)

