children => component => render
children可以是function也可以是其他.，比如component
children,render是function形式. component是组件
component 匿名函数的形式，效率低，会经常componentDidMount, componentWillUnmount
因为渲染component的时候，会调用React.createElment，每次都会生成不同
所有如果是匿名函数尽量使用render方式或者children. children,不管是否匹配都会显示

Switch， 找到第一个匹配的渲染


redux里的数据满足，共享的，该数据更新，所有组件都更新。 如果只有一个页面使用，那么不要存到redux中


redux saga
用于管理应用程序副作用的库，类比redux-thunk
监听takeEvery
调用异步操作 call， 
状态更新(dispatch) put 


1.创建 sagaMiddleware
2. 运行
worker saga 
watcher saga 监听



1.什么是vdom

js对象， 描述真实dom节点， 并且可以同步到真实dom. 修改js对象，这个过程叫协调也就是diff
比较两个新旧的js对象，得出一个最小的参数值，来更新真实的dom节点

2.为什么要使用vdom对象

为了实现最少的dom操作量。因为真实的dom节点，太大了，很多没用的属性。
DOM操作很慢，轻微的操作都肯能导致页面重新排版，非常消耗性能。相对于DOM对象，虚拟domjs对象
处理起来更快，而且更加简单。通过diff算法对比新旧vdom之间的差异，可以批量的，最小化的执行dom
操作，从而提高性能。
在哪里用vdom
react用jsx语法描述视图。该函数将生成vdom来描述真实dom。如果状态发生变化，那么vdom将做出相应变化，再通过diff算法对比新老的vdom区别从来做出最终dom操作。 17版本以上转义成了createElement.

JSX原理
16.X babelloader转义成createElement
17. 不会转换为react.createElement.而是自动从React的package中引入新的入口函数并调用。

与vue的区别
react中vdom+jsx一开始就有。 vue是后引入的。
vue把template编译为render函数的过程需要复杂的编译器转换字符串ast js函数字符串



什么是Fiber
Fiber单链表 vdom的一种实现形式
为什么需要fiber


reconciliation 协调（diff算法，diff过程）
核心就是diff.

diff策略
1.同级比较，web ui中DOM节点跨层级的移动操作特别少，可以忽略不计
是否可以复用，根据key和props，必须同一层级下
如果不能复用，那么老的删掉，新的新增。
不同类型的两个组件会生成不同的树形结构
2.拥有不同类型的两个组件将会生成不同的树形结构
3.开发者可以通过key, props 来暗示哪些元素在不同的渲染下能保持稳定。

diff过程
对比两个虚拟dom时，会有三种操作：删除，替换和更新
vnode和现在的虚拟dom， newVnode是更新虚拟dom
删除： newVnode不存在时
替换： vnode和newVnode类型不同或key不同时
更新： 有相同类型和key 但是vnode和newVnode不同时

vue和react Diff算法区别
1.数据结构不同。
react是单向链表
vue是数组双向查找（掐头掐尾）
2. react颗粒度小, 切分为一个一个小任务（虚拟dom）fiber


fiber的实现
window.requestIdlCallback

fiber的数据结构





===============================================================


Vue组件data为什么必须是个函数
init.js
mergeOptions
Vue组件可能存在多个实例，如果使用对象形式定义data,则会导致题目公用一个data的对象，那么状态变更将会影响所有组件实例（被污染），这是不合理；采用函数形式定义，在initData时将其作为工厂函数返回全新data对象，有效规避多实例之间状态污染问题。而在根实例创建过程中则不存在该限制，因为根实例只有一个 new Vue，不需要担心这种情况。
data选项检验。

vue的key作用和工作原理
patch.js updateChildren()
 samenode

1.key的作用主要是高效的更新虚拟dom。 原理是vue在patch过程中会执行patchVnode, patchVnode的时候会执行updateChildren方法，它会去更新所有的新旧两个子元素， 在这个过程中samenode函数通过KEY可以精准的判断两个节点是否同一个节点。如果没有
key会被当做相同的节点，加了key， 猜测首尾结构的相似性，可以高效的结束循环，减少dom操作，提升性能。
2. 隐蔽的bug
3.没有做唯一性判断，动画效果可能不会生效

vue中的diff 算法

lifecle.js
mountComponent $mount  一个组件一个watch
组件中可能存在很多个data中的key使用  

patch.js  patchVnode diff 发生的地方
整体策略： 深度优先，同层比较 (递归)
比孩子，比自己  updateChildren （核心）


vue 性能优化
1. 路由懒加载
2. keep-alive 缓存页面
3. v-show复用DOM
4. v-for 遍历避免同时使用v-if
5. 长列表性能优化，虚拟滚动
6. 事件销毁
7. 图片懒加载
8. 插件按需引入
9. 无状态的组件标记为函数式组件 functional
10.子组件分割
11. 变量本地化， 少引用this.XXX. 用变量缓存起来
12. ssr


vue3 新特性
静态属性提升，静态树提升， proxy, treeShaking, composition api, ts支持， reactivity（独立的响应式）


vue响应式的理解
1.啥是响应式
2.为什么需要
3.


vue组件之间的通信方式
1.父子组件 props $emit/$on(子与父) $parent/$children（直接引用）  ref, $attrs/$listenser（非属性的传递）
2.兄弟组件 （找共同的父亲，共同的根） $parent $root eventbus vuex
3.跨层级 eventbus, vuex, provide/inject(单向的)

eventbus
vuex

vuex的使用理解



function Add() {
 const nums = [...arguments];
 return () => {
  nums.push(...arguments);
  return () => {
   nums.push(...arguments);
   return nums.reduce((a, b) => a + b);
  }
 }
}


