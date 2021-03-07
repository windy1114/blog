### react合成事件原理
- ![事件委托](https://zh-hans.reactjs.org/static/bb4b10114882a50090b8ff61b3c4d0fd/31868/react_17_delegation.png)

### react合成事件和原生事件的区别

- 合成事件采用小驼峰命名，而不是纯小写

- jsx里面传入一个函数作为事件处理函数，而不是字符串

- 不能通过返回false来阻止默认行为，必须显示的用preventDefault

- 不需要使用addEventListener为已创建的dom元素添加监听器

- react class组件里，默认不会绑定this (手动绑定或者使用es6箭头函数)

  