vue的实现思想
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



