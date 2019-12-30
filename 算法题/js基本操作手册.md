##### # 复制得到一个新数组

```js
a = [1,2,3,4];
b = a.slice();
a === b; // false
```

⚠️ 从 array-move npm包中学习

git：https://github.com/sindresorhus/array-move

```js
const arrayMoveMutate = (array, from, to) => {
	array.splice(to < 0 ? array.length + to : to, 0, array.splice(from, 1)[0]);
};

const arrayMove = (array, from, to) => {
	array = array.slice();
	arrayMoveMutate(array, from, to);
	return array;
};
```

