### react中State
#为什么直接修改state不能生效，而要调用setState
多次setState不会触发多次渲染，并且state的值也不是实时的，这样的作法能够减少不必要的性能消耗。这个行为是通过批量更新来实现的。
#setState什么时候是同步的，什么时候是异步的
#setState是如何更新的
#state的更新为什么会被合并

react 的更新机制有关系，他自己写了一个类似异步更新数据的方法，调用setState  然后会把数据放在一个准备更新数据的链表里面，每个要更新的数据还会生成一个 利用时间计算出来的 戳
然后 后面再遍历这个链表 对数据进行更新，直接改state 数据没有没有放在链表里面


### class 组件的状态state和生命周期

componentDidMount //组件挂载完成
componentWillUnmount //组件卸载之前执行

useEffect 相当于三个生命周期的 componentDidMount, componentDidUpdate, componentWillUnmount

useEffect 有一个回调函数， 在这个函数里面可以执行clearInterval

### 生命周期
https://yuchengkai.cn/docs/frontend/react.html#react-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%88%86%E6%9E%90
用于组件不同阶段自定义功能。

shouldComponentUpdate() //此处可以做性能优化，避免render


componentWillUnmount //取消订阅，如clearInterval

componentWillReceiveProps //初次渲染的时候不会执行，只有在已挂载的组件接收新的props的时候，才会执行。

useMemo, useCallback, PureComponent  (函数每次执行会当作不一样)

useMemo和useCallback的区别

hook和高阶组件


为什么要引入react
如果使用jsx 就需要引入react， jsx需要babel-loader转译，最终要执行react.createElement()


this.forceupdate 

redux 数据流管理
redux的流程
react-redux
Provider 避免每个页面引入store
connect 不引入store的情况下获取数据和dispatch
reducer修改规则定义state的初始化
















整理的面试相关问题
https://github.com/lf2021/Front-End-Interview/blob/master/09.%E9%9D%A2%E8%AF%95%E5%A4%8D%E7%9B%98/%E7%A7%8B%E6%8B%9B%E9%9D%A2%E8%AF%95%E5%A4%8D%E7%9B%98.md#%E4%B8%80%E9%9D%A2-2020827
https://github.com/lf2021/Front-End-Interview/blob/master/09.%E9%9D%A2%E8%AF%95%E5%A4%8D%E7%9B%98/%E7%A7%8B%E6%8B%9B%E9%9D%A2%E8%AF%95%E5%A4%8D%E7%9B%98-yjj.md
https://github.com/CavsZhouyou/Front-End-Interview-Notebook/blob/master/JavaScript/JavaScript.md#35-javascript-%E7%BB%A7%E6%89%BF%E7%9A%84%E5%87%A0%E7%A7%8D%E5%AE%9E%E7%8E%B0%E6%96%B9%E5%BC%8F vue的生命周期
https://github.com/lf2021/Front-End-Interview/blob/master/08.%E9%9D%A2%E8%AF%95%E9%AB%98%E9%A2%91%E6%89%8B%E6%92%95%E4%BB%A3%E7%A0%81%E9%A2%98/%E9%9D%A2%E8%AF%95%E9%AB%98%E9%A2%91%E6%89%8B%E6%92%95%E4%BB%A3%E7%A0%81%E9%A2%98.md

https://github.com/jirengu/frontend-interview/issues?page=2&q=is%3Aissue+is%3Aopen

react面试题相关
https://mp.weixin.qq.com/s?__biz=Mzg2NDAzMjE5NQ==&mid=2247484667&idx=1&sn=dcaea6836c604100f9811c8c7f98a147&scene=21#wechat_redirect

深入系列
https://github.com/mqyqingfeng/Blog
https://github.com/mqyqingfeng/Blog/issues/2
https://juejin.im/user/712139233840407
react源码学习
https://github.com/KieSun/learn-react-essence/blob/master/%E7%83%AD%E8%BA%AB%E7%AF%87.md
promise原理的不错的文章
https://github.com/KieSun/learn-react-essence/blob/master/%E7%83%AD%E8%BA%AB%E7%AF%87.md


react 源码
build your own react

concurrent mode 并发模式


Step I: The createElement Function
Step II: The render Function
Step III: Concurrent Mode （react的并发模式）
Step IV: Fibers （）
Step V: Render and Commit Phases (render决定本次更新需要更新哪些东西， commit 把需要更新的内容更新到视图上)
Step VI: Reconciliation （render 阶段经历的 协调 也就是diff 算法，）
Step VII: Function Components （jsx组件承载，写函数组件）
Step VIII: Hooks （改变函数组件的状态）

react源码工作流程分为三个部分：
schedule 调度 产生的更新有不同的优先级 scheduler  独立的包 平台无关  优先级队列 小顶堆
render 协调 高优先级先进入render  reconciler  fiber 独立的包 平台无关      深度优先遍历 dfs  单向链表   lane模型 二进制 fiber架构
commit 渲染 改变视图进入commit renderer  独立的包平台相关 ReactDOM(浏览器) ReactNative  ReactArt(canvas)    

dispatchAtion