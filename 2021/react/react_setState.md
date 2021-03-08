### setState
setState 并不是单纯同步/异步的，它的表现会因调用场景的不同而不同：在 React 钩子函数及合成事件中，它表现为异步；而在 setTimeout、setInterval 等函数中，包括在 DOM 原生事件中，它都表现为同步。这种差异，本质上是由 React 事务机制和批量更新机制的工作方式来决定的。

- setState异步更新的原因是避免频繁的re-render
- setState异步的实现方式有点类似于vue的$nextTick和浏览器里的event-loop.
每来一个setState，就把它塞进一个队列里“攒起来”。等时机成熟，再把攒起来的state结果做合并，最后只针对最新的state值走一次更新流程。这个过程，叫作“批量更新”
- 对于相同属性的设置，React 只会为其保留最后一次的更新。并且它的值不会马上更新
- setTimeout里面setState是同步的，因为在setTimeout里面的setState脱离了react的管控。

#### setState的工作流程
setState->enqueueSetState->enqueueUpState->isBatchingUpdates?->是->组件入队dirtyComponents
setState->enqueueSetState->enqueueUpState->isBatchingUpdates?->否->循环更新dirtyComponents里的所有组件

#### React 中的 Transaction（事务）机制
