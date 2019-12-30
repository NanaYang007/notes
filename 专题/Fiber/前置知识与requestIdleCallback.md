#### 卡吗？

网页是浏览器一帧一帧绘制出来的，通常认为FPS（Frames Per Second）为60的时候比较流畅，FPS为个位数的时候就属于用户可以感知的卡顿了。

大多数设备的刷新频率是60次/秒，也就说是浏览器对每一帧画面的渲染工作要在16ms内完成，超出这个时间，页面的渲染就会出现卡顿现象，影响用户体验。



#### 浏览器一帧里做了什么

![图片描述](https://segmentfault.com/img/bV8O4y?w=2000&h=651)

如果这六个步骤中，任意一个步骤所占用的时间过长，总时间超过 16ms 了之后，用户也许就能看到卡顿。



* **Input events**

TouchEvent.type属性来确定当前事件属于哪种类型

>**注意:** 在很多情况下，触摸事件和鼠标事件会同时被触发（目的是让没有对触摸设备优化的代码仍然可以在触摸设备上正常工作）。如果你使用了触摸事件，可以调用 [`event.preventDefault()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault) 来阻止鼠标事件被触发。

https://developer.mozilla.org/zh-CN/docs/Web/API/TouchEvent

![图片描述](https://segmentfault.com/img/bV8O5A?w=717&h=310)

#### requestIdleCallback里面放什么？

执行DOM修改？ ❌ No

requestIdleCallback回调的执行说明前面工作（包括样式变更以及布局计算）都已完成。如果我们在callback里面做DOM修改的话，之前所做的布局计算都会失效，而且如果下一帧里有获取布局（如getBoundingClientRect、clientWidth）等操作的话，浏览器就不得不执行强制重排工作,这会极大的影响性能，另外由于修改dom操作的时间是不可预测的，因此很容易超出当前帧空闲时间的阈值，故而不推荐这么做。**推荐做法** 是在requestAnimationFrame里面做dom的修改，可以在requestIdleCallback里面构建Document Fragment，然后在下一帧的requestAnimationFrame里面应用Fragment。

Promise回调？ ❌ No

Promise的resolve(reject)操作也不建议放在里面，因为Promise的回调会在idle的回调执行完成后立刻执行，会拉长当前帧的耗时，所以不推荐。

建议放 ✅ 小块可预测时间

推荐放在requestIdleCallback里面的应该是小块的（microTask）并且可预测时间的任务。关于microTask推荐看：https://juejin.im/entry/59082301a22b9d0065f1a186



#### requestAnimation  vs.  requestIdleCallback

requestAnimationFrame的回调会在每一帧确定执行，属于高优先级任务，而requestIdleCallback的回调则不一定，属于低优先级任务。



console执行：

```js
requestIdleCallback((deadline)=>{
    console.log(deadline.timeRemaining(), deadline.didTimeout)
});
```

如果我们现在打开浏览器控制台，并执行上面的代码，Chrome会返回49.9， false。它告诉我们有49.9毫秒时间做需要做的任何工作，并且还没有用完分配的时间；否则deadline.didTimeout的值将会true。请记住，deadline.timeRemaining() 可以在浏览器完成某些工作后立即更改，因此应该不断检查。



