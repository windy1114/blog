vue的实际思想
1.数据驱动
2. mvvmm模式的践行者

mvvm的三个要素： 响应式， 模板引擎， 渲染
响应式： vue如何监听数据变化
模板： vue的模板如何编写和解析
渲染： vue如何把模板转换为html

create
组件实例已创建，还没有挂载， DOM还不存在 看不到this.$el

mounted
已挂载 将vdom转为了dom

beforeUpdate(当data被修改时) 和 updated（vdom重新渲染） 初始化阶段是不会执行的。 只有当数据更新的时候才会触发

beforeDestroy
destroied

Vue.set 别名 this.$set
动态修改对象 比如给对象添加属性 ， 并且显示到视图

Vue.delete  别名 this.$delete
删除对象的属性 如果对象是响应式的，确保删除能触发更新视图

vm.$on 监听当前实例上的自定义事件。事件可以由vm.$emit触发
vm.$emit 子组件给父组件派发事件
vm.$once
vm.$off

事件的派发和监听是同一个实例
【事件总线】 通过再Vue原型上添加一个vue实例作为事件总线，实现组件件相互通信，而且不受组件件关系的影响
Vue.propotype.$bus = new Vue()
这样做可以在任意组件中使用this.$bus访问到该Vue实例  (跨级通信)


ref 和 vm.$refs

ref是作为渲染结果被创建的，在初始渲染时不能访问他们 mounted里访问

$refs 不是响应式的，不要视图用它在模板中做数据绑定
当v-for用于元素或组件时，引用信息将时包含DOM节点或组件实例的数据

如果URL以~开头会作为一个模块请求被解析。 这意味着甚至可以引用node模块中的资源
如果URL以@开头会作为一个模块请求被解析。VUE cli默认会设置一个指向src的别名

通过webpack的处理
脚本和样式表会被压缩且打包在一起，从而避免额外的网络请求
文件丢失会直接在编译时报错，而不是到了用户端才产生404错误
最终生成的文件名包含了内容哈希，因此不必担心浏览器会缓存他们的老版本

用name的方式来传递路由


watch : {
	$route: {
	immediate: true,
	handler() {

}
}
}

this.$route.query.redirect

beforeEnter beforeEach beforeRouteEnter



面试题：
组件库相关问题
项目自己搭的？如何支持 treeshaking
如何做版本号管理
less 样式如何做按需加载
webpack 项目如何优化
ts 泛型
怎么通过实例拿到构造函数
extend 原理
Object.create 原理
虚拟列表原理
浏览器缓存原理
什么 csrf 攻击
csrftoken 怎么获取，存到哪里
并发调度手写题

call bind new 实现原理
vue 双向绑定原理
LRU 算法
http2.0 的有哪些内容
http 缓存
rem vw 区别
移动 1px 问题
函数柯里化
diff 算法
虚拟 dom
nextTick 原理
事件循环
闭包
如何解决移动端 click300ms 延迟？
vue 有哪些全局组件
移动端如何完成拖拽功能？
防抖和节流的区别
一道逻辑题：有 5L 的桶和 3L 的桶，如何拿到 4L 的水

讲讲 React 生命周期
webpack 你是如何做优化的
浏览器缓存
react 性能优化
vue 如何做权限检验
讲讲 http2.0
你是如何做性能优化的
单元测试如何测试，代码覆盖率如何
react 生命周期
说说 react 状态逻辑复用问题
react fiber 节点（不会，没研究过）
Koa 中间件原理
Redux 工作流？
Koa 如何实现监控处理
如何实现 Redux 异步功能
Redux 如何优化
commonjs 的实现原理
讲讲垃圾回收机制
Vue 和 React 的区别
函数式编程 如何理解纯函数
Node 原生 api 错误处理有了解吗
说说浏览器渲染流程
说说重绘和重排
说说那些属性可以直接避免重绘和重排
treeshaking 原理
按需加载的原理
讲讲原型链
了解过那些前端构建工具 分别介绍他 webpack rollup gulp
双向数据绑定原理
说 vue 如何收集依赖的

https://juejin.im/post/6874275613360783368#heading-28

https://www.cxymsg.com/guide/react.html#mixin%E3%80%81hoc%E3%80%81render-props%E3%80%81react-hooks%E7%9A%84%E4%BC%98%E5%8A%A3%E5%A6%82%E4%BD%95%EF%BC%9F


//
算法：实现36进制转换
简述https原理，以及与http的区别
操作系统中进程和线程怎么通信
node中cluster是怎样开启多进程的，并且一个端口可以被多个进程监听吗
实现原生ajax
vue-router源码
vue原理（手写代码，实现数据劫持）
算法：树的遍历有几种方式，实现下层次遍历
算法：判断对称二叉树

介绍一下项目中的难点
let var const 有什么区别
你知道哪些http头部
怎么与服务端保持连接
http请求跨域问题，你都知道哪些解决跨域的方法
webpack怎么优化
你了解哪些请求方法，分别有哪些作用和不同
你觉得typescript和javascript有什么区别
typescript你都用过哪些类型
typescript中type和interface的区别
react怎么优化
算法题：合并乱序区间


回答http头部的时候，顺带跟面试官聊到了浏览器缓存，回答跨域的时候，面试官又
让我用jsonp实现一下跨域，回答webpack的时候提到了happypack和treeshaking，面试官就
顺带问了一下他们的作用

你了解node多进程吗
node进程中怎么通信
node可以开启多线程吗
算法题：老师分饼干，每个孩子只能得到一块饼干，但每个孩子想要的饼干大小不尽相同。
目标是尽量让更多的孩子满意。 如孩子的要求是 1, 3, 5, 4, 2，饼干是1, 1，
最多能让1个孩子满足。如孩子的要求是 10, 9, 8, 7, 6，饼干是7, 6, 5，最多能
让2个孩子满足。
算法题：给定一个正整数数列a, 对于其每个区间, 我们都可以计算一个X值;
X值的定义如下: 对于任意区间, 其X值等于区间内最小的那个数乘上区间内所有数和;
现在需要你找出数列a的所有区间中, X值最大的那个区间;
如数列a为: 3 1 6 4 5 2; 则X值最大的区间为6, 4, 5, X = 4 * (6+4+5) = 60;

算法题：两个有序链表和并成一个有序链表
https与http有什么区别(一面刚好也被问到)
cookie有哪些属性
cookie,session,localstorage,sessionstorage有什么区别
怎么禁止js访问cookie
position有哪些属性
你知道哪些状态码
options请求方法有什么用
less,sass它们的作用是什么


https://juejin.im/post/6854899692178948109#heading-10

https://github.com/lgwebdream/FE-Interview/issues/12

https://juejin.im/post/6844903959736352782  原生 js实现一个前端路由。
https://juejin.im/post/6844903935895928839#heading-9 webpack打包原理
https://juejin.im/post/6844903655619969038  http缓存


https://juejin.im/post/6844904106537009159 阿里淘系、阿里云、字节跳动面经 & 个人成长经验分享

https://juejin.im/post/6844904161830502407
蚂蚁、字节、滴滴面试经历总结

https://juejin.im/post/6844903896637259784 各种文章合集

https://juejin.im/post/6844904093425598471 阿里面试题合集

https://www.cxymsg.com/guide/react.html#mixin%E3%80%81hoc%E3%80%81render-props%E3%80%81react-hooks%E7%9A%84%E4%BC%98%E5%8A%A3%E5%A6%82%E4%BD%95%EF%BC%9F react面试题

https://juejin.im/post/6844904121380667399 五万字面试宝典

https://juejin.im/user/2858385961407853/posts 前端有的玩

https://juejin.im/post/6844904088790892551 阿里头条面经
https://juejin.im/post/6844904085183791117 字节面试
https://juejin.im/post/6844904095564709896 头条
https://juejin.im/post/6866082181455249422 头条
https://juejin.im/post/6844904086358212621 P7的面经

https://juejin.im/post/6844904100081958919#heading-4 各种原理
https://juejin.im/post/6867330563943071758#heading-3 字节


code地址 ：github： https://github.com/57code/vue-study.git
                    码云：https://github.com/57code/vue-study.git
 web23分支