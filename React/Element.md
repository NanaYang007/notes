一个Element

```js
{
  type: 'button',
  props: {
    className: 'button button-blue',
    children: {
      type: 'b',
      props: {
        children: 'OK!'
      }
    }
  }
}
```

代表：

```html
<button class='button button-blue'>
  <b>
  	OK!
  </b>
</button>
```



React element很轻量，因为他们只是Object



##### React 核心

一个描述component的element也是一个element，就像是一个描述DOM节点的element。它们可以嵌套和混合。

* 嵌套

```js
const DangerButton = ({ children }) => ({
  type: Button,
  props: {
    color: 'red',
    children: children
  }
})
```

* 混合

```js
const DeleteAccount =() => ({
  type: 'div',
  props: {
    children: [{
      type: 'p',
      props: {
        children: 'Are you sure?'
      }
    }, {
      type: DangerButton,
      props: {
        children: 'Yep'
      }
    }, {
      type: Button,
      props: {
        color: 'blue',
        children: 'Cancel'
      }
    }]
  }
})
```

等同于JSX：

```js
const DeleteAccount = () => (
	<div>
  	<p>Are you sure?</p>
  	<DangerButton>Yep</DangerButton>
  	<Button color='blue'>Cancel</Button>
  </div>
)
```

