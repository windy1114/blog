## react性能优化

### 浅比较深比较

### PureComponent
React.PureComponent用当前和之前props,state的浅比较覆写了shouldComponentUpdate的实现

### React.memo
使用 PureComponent 或是 shouldComponentUpdate 来解决 class 组件在 props 不变时会重新渲染的问题，使用React.memo包裹函数组件使其获得同样的能力

### shouldComponentUpdate
shouldComponentUpdate会在渲染前被触发。默认返回true
```
shouldComponentUpdate(nextProps, nextState) {
	return true;
}
```
### React.lazy 
依赖Suspense进行代码分割。 通过React.lazy() 包装动态加载的组件的方式使用Suspense来进行 代码分割了。
```
import React, {lazy, Suspense} from 'react';
const OtherComponent = lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <OtherComponent />
    </Suspense>
  );
}
``` 

### props vs state
https://lucybain.com/blog/2016/react-state-vs-pros/
- prop和state 都是一个js对象
- props和state的改变都会引起组件更新
- props are a Component's configuration, props是可选的
- 组件不能修改它的props。but it is responsible for putting together the props of its child Components.
- state可以说是组件私有的
- 不要把props赋值给state, 因为props更新之后，state不会被更新。 任何数据，都要保证只有一个数据来源，而且避免直接复制它。

|	props |	state | | |
| ---- | ----|----| ----|
| Can get initial value from parent Component? |	Yes |	Yes|
| Can be changed by parent Component? |	Yes |	No |
| Can set default values inside Component?* |	Yes |	Yes |
| Can change inside Component? |	No |	Yes |
| Can set initial value for child Components? |	Yes |	Yes|
| Can change in child Components? |	Yes |	No |


### react render
####react render做了什么事
react的render()方法，会创建一颗由react元素组成的树，在下一次state或props更新时，相同的render方法会返回一棵不同的树。react需要基于这两颗树之间的差别来判断如何有效率的更新UI以保证当前UI与最新的树保持同步
渲染阶段的生命周期包括以下 class 组件方法：
- constructor
- componentWillMount (or UNSAFE_componentWillMount)
- componentWillReceiveProps (or UNSAFE_componentWillReceiveProps)
- componentWillUpdate (or UNSAFE_componentWillUpdate)
- getDerivedStateFromProps
- shouldComponentUpdate
shouldComponentUpdate()返回false那么不会执行render()
- render
- setState 更新函数（第一个参数）

#### react 启发式算法的两个假设：
1. 两个不同类型的元素会产生不同的树
2. 开发者可以通过key prop来暗示哪些子元素在不同的渲染下能保持稳定

### react diff
时间复杂度是O(n)
- 对比不同类型的元素
当根节点是不同类型的树，React会拆卸原有的树，并且建立新的树。 拆卸一棵树时，对应的DOM节点也会被销毁。组件实例将会执行componentWillUnmount()。当创建一颗新树的时候，对应的DOM节点会被创建以及插入到DOM中。组件实例将执行UNSAFE_componentWillMount(),和componenDidMount方法。所有跟之前的树所关联的state也会被销毁。在根节点以下的组件也会被卸载，它们的状态会被销毁。
- 对比同一类的元素
当对比两个相同类型的React元素时，React会保留DOM节点，仅对比及更新所有改变的属性。在处理完当前节点之后，React 继续对子节点进行递归。
- 对比同类型的组件元素React 
当一个组件更新时，组件实例保持不变，将更新该组件实例的 props 以跟最新的元素保持一致，并且调用该实例的 UNSAFE_componentWillReceiveProps()、UNSAFE_componentWillUpdate() 以及 componentDidUpdate() 方法。下一步，调用 render() 方法，diff 算法将在之前的结果以及新的结果中进行递归。
- 对比子元素
当递归 DOM 节点的子元素时，React 会同时遍历两个子元素的列表；当产生差异时，生成一个 mutation。


### 受控组件vs非受控组件
- 推荐使用受控组件来处理表单数据，受控组件中，表单数据就由react来管理的。非受控组件的表单数据由DOM节点来处理
- 非受控组件中表单设置默认值用defaultValue而不是value。组件挂载之后更新defaultValue的值，不会造成DOM上值的任何更新。
- <input type="file" /> 始终是一个非受控组件，因为它的值只能由用户设置，而不能通过代码控制
- 受控组件中，输入的值始终由React的state来驱动。

### 状态提升
多个组件需要反映相同的变化数据，这时我们建议将共享状态提升到最近的共同父组件中去

