## Vue的核心思想
数据驱动试图 ui = fn(state)

## new Vue之后发生了什么事情
```
	function Vue (options) {
		if (process.env.NODE_ENV !== 'production' && !(this.instanceof Vue)) {
			warn('Vue is a constructor and should be called with the `new` keyword)
		}
		this._init(options)
	}

```
new Vue(options: Object) 实例化Vue
Vue实际上是一个类, 用function来实现的. 只能通过new来初始化, 之后会调用 this._init(options)
init() 又做了合并配置项，初始化生命周期钩子（beforeCreate， created），初始化事件中心，初始化渲染，初始化data, props,computed, watcher等等。 最后调用vm.$mount(vm.$options.el) 挂载vm, 挂载的目的是把模板渲染成最终的DOM



## 模板和数据如何渲染成最终的DOM？

初始化的时候调用vm.$mount(vm.$options.el) 挂载vm，把模板渲染成最终的DOM

## Vue实例挂载`$mount`的实现
以下分析带compiler版本的$mount
- Vue不能挂载在body, html上
- 如果没有renader函数，那么就把template/el 转化为render 函数

```js
Vue.prototype.$mount = function (
  el?: string | Element,
  hydrating?: boolean): Component {
  el = el && inBrowser ? query(el) : undefined
  return mountComponent(this, el, hydrating)
}

```
### $mount 时间上是调用mountComponent
mountComponent的核心是先实例化一个渲染watcher，在它的回调函数中会调用 updateComponent 方法， 在次方法中调用 vm._render() 方法先生成虚拟Node,最终调用 vm._update 更新DOM

### Vue 2.x  中组件的渲染最终都是通过render方法，这个过程实际是Vue的一个"在线编译"的过程，它是调用compileToFunctions方法来实现的。

## 整个过程就是：
new Vue -> init -> $mount -> compile -> render -> vnode -> patch -> dom


## Vue 响应式原理

### 当修改数据，模板对应的插值也会渲染成新的数据，那这一切是怎么做到的呢
#### 自己来实现， 监听点击事件，修改数据，手动操作DOM重新渲染
- 需要考虑以下问题：
- 需要修改哪块的DOM
- 修改的效率是不是最优的
- 需要对数据每一次的修改都去操作DOM吗
- 需要case by case去修改DOM的逻辑吗

#### Vue的实现原理 Object.defineProperty



https://ustbhuangyi.github.io/vue-analysis/v2/reactive/reactive-object.html#object-defineproperty
