### React合成事件原理
React16和以下版本中的事件机制并不是原生的那一套，事件没有绑定在原生DOM上，大多数事件都绑定在document上（除了少数不会冒泡到document的事件，如scroll）。

### React17 事件机制的更新
- ![事件委托](https://zh-hans.reactjs.org/static/bb4b10114882a50090b8ff61b3c4d0fd/31868/react_17_delegation.png)
- 废除了事件池

### React合成事件和原生事件的区别

- 合成事件采用小驼峰命名，而不是纯小写

- jsx里面传入一个函数作为事件处理函数，而不是字符串

- 不能通过返回false来阻止默认行为，必须显示的用preventDefault

- 不需要使用addEventListener为已创建的dom元素添加监听器

- React class组件里，默认不会绑定this (手动绑定或者使用es6箭头函数)
- e.stopPropagation不能组件原生事件冒泡，因为事件早已经冒泡到document上，React在事件冒泡到document上时才能够处理事件。
- 原生事件早于合成事件
- 通过合成事件上的nativeEvent可以访问原生事件
  