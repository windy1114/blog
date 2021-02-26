## this到底指向哪里呢
- 非显示或隐式调用的简单调用函数时，在严格模式下，this指向undefined. 非严格模式下指向window/global上
- 使用new方法调用构造函数时，this会指向新创建的对象上
- 通过上下文对象调用函数时，函数体内的this会指向该对象， 指向最后调用它的对象
- 普通函数体在显示调用中，比如call/apply/bind. this指向的是指定参数的对象上。
- 箭头函数中，this指向由外层作用域来决定（也就是拥有自己this的外层）



#### 全局环境中的this

```javascript
function f1() {
    console.log(this) //window
}

function f2() {
    "use strict"
    console.log(this)  //undefined
}

```

#### 全局的变种

```
const foo = {
    bar: 1,
    fn: function () {
        console.log(this)
        console.log(this.bar)
    }
}
const fn1 = foo.fn
fn1()  //window, undefined
```



#### 上下文对象调用中的this

```

```

