## axios 源码分析
### axios是什么
axios是一个基于Promise的HTTP库。可以在浏览器和node.js中使用。在浏览器端使用XMLHttpRequest请求,在node.js中使用http发送请求。
### axios的工作原理
- axios为什么可以当函数调用，也可以当对象使用，如axios.get
- 拦截器的实现原理
- 中断请求是怎么实现的
- 怎么实现xsrf防范的


### axios的工作流程

### lib/axios.js
axios是Axios.prototype.request函数bind()返回的函数
### core/Axios.js
Axios是一个构造函数。 提供request方法，以及别名方法'delete', 'get', 'head', 'options'，'post', 'put', 'patch'
request返回一个promise. promise的链式调用绑定拦截器的回调函数
### core/dispatchRequest.js
发送请求， 返回promise
### defaults.js
 `getDefaultAdapter` 浏览器端使用XMLHttpRequest. node端使用http
### adapters
- http.js
- xhr.js
### core/interceptorManager.js



https://github.com/lxchuan12/axios-analysis






