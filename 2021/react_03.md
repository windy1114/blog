## 组件生命周期
组件的灵魂是render()
#### 在react 15中的生命周期
- 初始化渲染 
constructor() -> componentWillMount()->render()->componentDidMount()->componentWillUnmount()
- 组件更新(父组件触发)
componentWillReceiveProps()->shouldComponentUpdate()->componentWillUpdate()->render()->componentDidUpdate()->componentWillUnmount()
- 组件更新(自身组件触发)
shouldComponentUpdate()->componentWillUpdate()->render()->componentDidUpdate()->componentWillUnmount()
- constructor()初始化state
- componentWillMount() 
- render() 返回需要渲染的内容,不会去操作真实 DOM
- componentDidMount() 真实dom已经挂载到页面上
- componentWillReceiveProps() 并不是由 props 的变化触发的，而是由父组件的更新触发的。
- componentWillUnmount() 组件销毁。 组件销毁的两个原因：组件在父组件中被移除；组件中设置了key，父组件在render过程中，发现key值和上次不一致。

#### 在react 16中的生命周期
- 初始化阶段
constructor()->getDerivedStateFromProps()->render()->componentDidMount()
- 更新阶段 父组件触发
getDerivedStateFromProps()->shouldComponentUpdate()->render()->getSnapshotBeforeUpdate()->componentDidUpdate()
- 更新阶段 自身触发
shouldComponentUpdate()->render()->getSnapshotBeforeUpdate()->componentDidUpdate()
- static getDerivedStateFromProps(props, state) 静态方
- getDerivedStateFromProps 是一个静态方法，没有this
- getDerivedStateFromProps 可以接受两个参数props(来自父组件)，state(自身)
- getDerivedStateFromProps 需要返回一个对象格式(对象或者null)，如果不，react会警告。因为react需要找个值来更新组件的state。它对 state 的更新动作并非“覆盖”式的更新，而是针对某个属性的定向更新。如果state不存在这个属性，那么state会增加这个属性，如果返回null或者{} 那么state还是原来的state
- getDerivedStateFromProps 触发的原因1. setState, 2. forceUpdate
- getDerivedStateFromProps 不能和componentWillReceiveProps划等号
- 只用 getDerivedStateFromProps 来完成 props 到 state 的映射.确保生命周期函数的行为更加可控可预测
- getSnapshotBeforeUpdate(prevProps, prevState) 的返回值会作为第三个参数给到componentDidUpdate。执行时机是render()之后，真实dom之前。
- getSnapshotBeforeUpdate 可以同时获取新旧props和state，还有更新前的真实dom
- getSnapshotBeforeUpdate 要想发挥作用，离不开 componentDidUpdate 的配合

#### Fiber机制下的生命周期分为render,pre-commit, commit阶段
- render阶段是可以被暂停，终止或同步执行的。是异步的。因为该阶段对于用户来说是不被感知的。
- pre-commit阶段可以读取DOM
- commit 阶段可以使用DOM，运行副作用，安排更新。commit阶段总是同步执行的，因为用户能感知到视图的变化。

#### 将被废弃的生命周期
componentWillMount,componentWillReceiveProps,componentWillUpdate。
废弃的原因：因为Fiber机制下这三个生命周期都处于render阶段。而render阶段是允许暂停、终止和重启的，所以会导致它们可能被重复执行，如果开发者使用不当，那么会有bug。如 componentWillMount 里发起异步请求。在 componentWillxxx 被打断 + 重启多次后，就会发出多个异步请求。



