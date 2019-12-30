**ExpirationTime的作用**
在`React`中，为防止某个`update`因为优先级的原因一直被打断而未能执行。`React`会设置一个`ExpirationTime`，当时间到了`ExpirationTime`的时候，如果某个`update`还未执行的话，`React`将会强制执行该`update`，这就是`ExpirationTime`的作用。



```js
case UserBlockingPriority:        
  // TODO: Rename this to computeUserBlockingExpiration        	 //一个是计算交互事件（如点击）的过期时间        
  expirationTime = computeInteractiveExpiration(currentTime);        
  break;      
case NormalPriority:      
case LowPriority: 
  // TODO: Handle LowPriority        
  // TODO: Rename this to... something better.        
  //一个是计算异步更新的过期时间        
	expirationTime = computeAsyncExpiration(currentTime);        	 break;
```

* 低优先级的过期时间**间隔**是`25ms`
* 高优先级的过期时间**间隔**是`10ms`

**也就是说，`React`低优先级`update`的`expirationTime`间隔是`25ms`， `React`让两个相近（`25ms`内）的`update`得到相同的`expirationTime`，目的就是让这两个`update`自动合并成一个`Update`，从而达到批量更新的目的，就像`LOW_PRIORITY_BATCH_SIZE`的名字一样，自动合并批量更新。**

想象一下，开发者不停地使用`setState()`更新`ReactApp`，如果不把相近的`update`合并的话，会严重影响性能，就像提到的`doubleBuffer`一样，`React`为提高性能，考虑得非常全面！





作者：进击的小进进链接：https://juejin.im/post/5d6a572ce51d4561fa2ec0bc





来一个setState:

```js
function computeExpirationForFiber(currentTime, fiber) {
  var priorityLevel = scheduler.unstable_getCurrentPriorityLevel();

  var expirationTime = void 0;
  if ((fiber.mode & ConcurrentMode) === NoContext) {
    // Outside of concurrent mode, updates are always synchronous.
    expirationTime = Sync;
  } else if (isWorking && !isCommitting$1) {
    // During render phase, updates expire during as the current render.
    expirationTime = nextRenderExpirationTime;
  } else {
    switch (priorityLevel) {
      case scheduler.unstable_ImmediatePriority:
        expirationTime = Sync;
        break;
      case scheduler.unstable_UserBlockingPriority:
        expirationTime = computeInteractiveExpiration(currentTime);
        break;
      case scheduler.unstable_NormalPriority:
        // This is a normal, concurrent update
        expirationTime = computeAsyncExpiration(currentTime);
        break;
      case scheduler.unstable_LowPriority:
      case scheduler.unstable_IdlePriority:
        expirationTime = Never;
        break;
      default:
        invariant(false, 'Unknown priority level. This error is likely caused by a bug in React. Please file an issue.');
    }

    // If we're in the middle of rendering a tree, do not update at the same
    // expiration time that is already rendering.
    if (nextRoot !== null && expirationTime === nextRenderExpirationTime) {
      expirationTime -= 1;
    }
  }

  // Keep track of the lowest pending interactive expiration time. This
  // allows us to synchronously flush all interactive updates
  // when needed.
  // TODO: Move this to renderer?
  if (priorityLevel === scheduler.unstable_UserBlockingPriority && (lowestPriorityPendingInteractiveExpirationTime === NoWork || expirationTime < lowestPriorityPendingInteractiveExpirationTime)) {
    lowestPriorityPendingInteractiveExpirationTime = expirationTime;
  }

  return expirationTime;
}
```

