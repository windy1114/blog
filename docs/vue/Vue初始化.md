## Vue初始化
![avatar](https://p2.ssl.qhimg.com/t01cebfb4c74c7fe96d.png)
### 1 入口文件
src\platforms\web\entry-runtime-with-compiler.js
扩展$mount，处理可能存在的templte或el选项 做了以下操作
- 1.获取template 模板字符串
获取$opitons 如果有render函数，执行render函数, 如果没有render函数查找template
对于template的处理，如果template是string,template是id选择器吗，如果是，获取选择器的内容。
如果template是元素，获取元素的innerHTML
这里解析了new Vue的时候render()>template>el的优先级。以及template选项可以是模板或者选择器。
- 2.编译template为render函数

### 2.runtime/index
实现一个__patch__函数
实现$mount

### 3.core/index
1.初始化全局api

### 4.core/instance/index.js
vue构造函数
1.声明构造函数
2.实现 实例属性，实例方法
	2.1 inintMixin  //实现_inint()方法
	2.2 stateMixin   //实现状态相关的方法$set $delete $watch $data $props $watch
	2.3 eventsMixin  //$emit $on $off $once
	2.4 lifeCycleMixin //_update() $forceUpdate() $destroy()
	2.5 renderMixin //$nextTick() _render()
4.1 .init.js
实现_init初始化方法 //new Vue都发生了什么
1.合并选型 mergeOptions  合并用户选型和全局filters,directives, data, components,__proto__(keep-alive, transitions, transitionsGroup) 
2.初始化核心代码
  initLifeCycle //$parent /$root/$children
  initEvent //自定义事件监听
  initRender //slot处理 实现createElement （render函数中的参数h函数）定义$attrs 和$listeners响应式
  callHook // beforeCreate
  initInjections //数据初始化 //隔代注入的data/props
  initState //数据初始化 props,methods,data, computed, watch state.js(解释了优先级 props?methods?data?computed?watch) 
  initProvide //数据初始化  data/props
  callHook //created
  
####  什么时候会调用$mount?
当设置了el选项时，自动调用$mount
#### new Vue都做了哪些事
- 派发生命周期钩子beforeCreated, created, 
- 初始化实例， 实例相关的各种属性如$parent $root $slot props,data,watch, computed数据响应式处理

