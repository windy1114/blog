## axios 源码分析
### axios是什么
axios是一个基于Promise的HTTP库。可以在浏览器和node.js中使用。在浏览器端使用XMLHttpRequest请求,在node.js中使用http发送请求。
### axios的实现原理
- axios为什么可以当函数调用，也可以当对象使用，如axios.get
- 拦截器的实现原理
- 中断请求是怎么实现的
- 怎么实现xsrf防范的
```
├── /lib/                      # 项目源码目
│ ├── /adapters/               # 定义发送请求的适配器
│ │ ├── http.js                # node环境http对象
│ │ └── xhr.js                 # 浏览器环境XML对象
│ ├── /cancel/                 # 定义取消功能
│ ├── /helpers/                # 一些辅助方法
│ ├── /core/                   # 一些核心功能
│ │ ├── Axios.js               # axios实例构造函数
│ │ ├── createError.js         # 抛出错误
│ │ ├── dispatchRequest.js     # 用来调用http请求适配器方法发送请求
│ │ ├── InterceptorManager.js  # 拦截器管理器
│ │ ├── mergeConfig.js         # 合并参数
│ │ ├── settle.js              # 根据http响应状态，改变Promise的状态
│ │ └── transformData.js       # 改变数据格式
│ ├── axios.js                 # 入口，创建构造函数
│ ├── defaults.js              # 默认配置
│ └── utils.js                 # 公用工具

```

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






