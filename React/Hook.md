**useState**

```react
function Example() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={()=>setCount(count+1)}>
    	Click me
    </button>
  );
}
```

避免一堆useState，也可以使用

```react
const [state, setState] = useState({a:1, b:'d',c: false});
```

https://reactjs.org/docs/hooks-faq.html#should-i-use-one-or-many-state-variables

---

**useEffect**

useEffect可以达到与class中的componentDidMount，componentDidUpdate和componentWillUnmount一样的效果

```react
// Similar to componentDidMount and componentDidUpdate
useEffect(() => {
  document.title = `You clicked ${count} times`;
});
```

默认条件下，React在每一次render后运行effects（包括第一次nreder）

