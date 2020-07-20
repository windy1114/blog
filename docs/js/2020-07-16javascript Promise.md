### Promise
https://github.com/codediodeio/async-await-pro-tips/blob/master/4-concurrency.ts

1.什么是Promise
2.Promise的优点 Promise 解决了回调地狱的基本缺陷

Promise是一个代理对象（代理一个值），它代表了一个异步操作的最终完成或者失败,以及其结果值。 
这个异步方法可以像同步方法那样返回值，单并不是立即返回最终执行结果，而是一个能代表未来出现的结果的promise对象

它的本质是一个函数返回的对象，可以在它上面绑定回调函数，这样就不需要在一开始把回调函数作为参数传入这个函数了。

## promise的状态
1. pending 始状态，既不是成功，也不是失败状态
2. fulfilled 意味着操作成功完成。
3. rejected 意味着操作失败。

## 参数
参数executor是一个带有resolve,和reject的函数。 Promise构造函数 执行时立即调用executor函数， resolve和reject两个函数
作为参数传递给executor（executor函数在Promise构造函数返回所建promise实例对象前调用）。
resolve和reject两个函数被调用时分别将promise的状态改为fulfilled 或 rejected。
eecutor内部通常会执行一些异步操作，一旦异步操作执行完毕（可能成功可能失败） 要么调用resolve函数将promise状态改为fullfiled 要么将promise的状态改为rejected的。
如果在executor函数中抛出一个错误，那么改promise状态为rejected. executor函数返回值被忽略。


构造函数主要用于包装还未支持promises的函数
```
new Promise( function(resolve, reject) {...} /* executor */  );
```
```
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('foo');
  }, 300);
});

promise1.then((value) => {
  console.log(value);
  // expected output: "foo"
});

console.log(promise1);

> [object Promise]
> "foo"
```

Promise 的有点链式调用

使用Promise时的约定
1.在本轮事件循环运行完成之前，回调函数是不会被调用的。
2.即使异步操作已经完成（成功或者失败）在这之后通过then()添加的回调函数也会被调用
3.通过多次调用then() 可以添加多个回调函数，它们会按照插入顺序执行

链式调用解决的问题
连续执行两个或者以上的异步操作，在上一个操作执行成功之后，并将上一个操作的返回结果作为参数去开始下一个操作。可以通过创建一个Promise链来实现
then 函数会返回一个和原来不同的新的Promise

const promise1 = doSomething()
const promise2 = promise1.then(successCallback, failtureCallback)
或者
const promise2 = doSomething().then(successCallback, failtureCallback)

promise2 不仅表示 doSomething()的完成，也代表了传入的successCallback, 或者 failtureCallback的完成。 这两个函数也会返回新的Promise对象， 从而形成一个异步操作。 这样在promise2上新增的回调函数会排在这个promise对象的后面

每个promise对象都代表了链中另一个异步过程的完成

在过去，要想做多重的异步操作，会导致经典的回调地狱，现在，我们可以把回调绑定到返回的 Promise 上，形成一个 Promise 链：
```
doSomething().then(function(result) {
  return doSomethingElse(result);
})
.then(function(newResult) {
  return doThirdThing(newResult);
})
.then(function(finalResult) {
  console.log('Got the final result: ' + finalResult);
})
.catch(failureCallback);
```

then里的参数是可选的，catch(failureCallback) 是 then(null, failureCallback) 的缩略形式。如下所示，我们也可以用箭头函数来表示


```
doSomething()
.then(result => doSomethingElse(result))
.then(newResult => doThirdThing(newResult))
.then(finalResult => {
  console.log(`Got the final result: ${finalResult}`);
})
.catch(failureCallback);
```
一定要有返回值，否则，callback将无法获取上一个Promise的结果。
(如果使用箭头函数，() => x 比 () => { return x; } 更简洁一些，但后一种保留 return 的写法才支持使用多个语句。）。

catch的后续链式操作
有可能在一个回调失败之后继续使用链式操作，即使用一个catch，这对于在链式操作中抛出一个失败之后，再次进行新的操作很有用。

```
new Promise((resolve, reject) => {
    console.log('初始化');

    resolve();
})
.then(() => {
    throw new Error('有哪里不对了');
        
    console.log('执行「这个」”');
})
.catch(() => {
    console.log('执行「那个」');
})
.then(() => {
    console.log('执行「这个」，无论前面发生了什么');
});
```

输出结果如下：
初始化
执行“那个”
执行“这个”, 无论前面发生了什么

因为抛出了错误 <u>有哪里不对了</u>，所以前一个 <u>执行【这个】没有被输出</u>

通常，一遇到异常抛出，浏览器就会顺着promise链寻找下一个onRejected 失败回调函数或者由.catch()指定的回调函数。
和下边这个同步代码的执行过程很相似。

```
try {
  let result = syncDoSomething();
  let newResult = syncDoSomethingElse(result);
  let finalResult = syncDoThirdThing(newResult);
  console.log(`Got the final result: ${finalResult}`);
} catch(error) {
  failureCallback(error);
}

```

在 ECMAScript 2017 标准的 async/await 语法糖中，这种与同步形式代码的对称性得到了极致的体现：
```
async function foo() {
  try {
    const result = await doSomething();
    const newResult = await doSomethingElse(result);
    const finalResult = await doThirdThing(newResult);
    console.log(`Got the final result: ${finalResult}`);
  } catch(error) {
    failureCallback(error);
  }
}
```


Promise的拒绝事件
当promise被拒绝时，会有rejectionhandled和unrejectedrejection两个事件中的一个被派发到全局作用域（一般时window,如果时web work中使用，就是worker 或者是其他worker-based接口）

rejectionhandled
当Promise被拒绝，并且再reject函数处理改rejection之后会派发次事件
unhandlerejection
当promise被拒绝，但是没有提供reject函数来处理该rejection时，会派发此事件。

以上两种情况种，PromiseRejectionEvent事件都有两个属性，一个时promise属性，该属性指向被驳回的Promise，另一个是reason属性，该属性用来说明Promise被驳回的原因。

我们可以通过以上事件为Promise失败时提供补偿处理，也有利于调试Promise相关的问题。在每一个上下文中该处理都是全局的。因此不管源码如何，所有的错误都会在同一个handler中被捕捉处理。

一个特别有用的例子：当你使用 Node.js 时，有些依赖模块可能会有未被处理的 rejected promises，这些都会在运行时打印到控制台。你可以在自己的代码中捕捉这些信息，然后添加与 unhandledrejection 相应的 handler 来做分析和处理，或只是为了让你的输出更整洁。举例如下：
```
window.addEventListener("unhandledrejection", event => {
  /* 你可以在这里添加一些代码，以便检查
     event.promise 中的 promise 和
     event.reason 中的 rejection 原因 */

  event.preventDefault();
}, false);
```
调用 event 的 preventDefault() 方法是为了告诉 JavaScript 引擎当 promise 被拒绝时不要执行默认操作，默认操作一般会包含把错误打印到控制台。

理想情况下，在忽略这些事件之前，我们应该检查所有被拒绝的 Promise，来确认这不是代码中的 bug。


Promise.resolve和Promise.reject是手动创建一个已经resolve或者reject的Promise快捷方法。

Promise.all 和Promise.race 是并行运行异步操作的两个组合式工具

我们可以发起并行操作，然后等多个操作全部结束后进行下一步的操作。

```
Promise.all([function1, function2, function3])
.then(([result1, result2, result3])=>{/* use result1, result2, result3*/})

```
通常我们递归调用一个由异步函数组成的数组时，相当于执行了一个Promise链
```
Promise.resolve().then(function1).then(function2).then(function3)

```
在 ECMAScript 2017 标准中, 时序组合可以通过使用 async/await 而变得更简单：

```
let result
for(const f of [function1, function2, function3]) {
	result = await f(result)
}
/* use last reslut (i.e. result3)*/

```

## 时序

即便一个已经变成resolve状态的Promise，传递给 then() 的函数也总是会被异步调用的。、
```
Promise.resolve().then(()=> console.log(2));
console.log(1) //1,2

```
传递到then()中的函数被置入了一个微任务队列，而不是立即执行，这意味着它时在js事件队列的所有运行时结束了，事件队列被清空之后才开始执行的。

## 常见错误
1.忘记终止返回一个Promise （经验法则是总是返回或者终止Promise链，并且一旦你得到一个新的Promise返回它。）
2.不必要的嵌套
3.忘记用catch终止链 这货导致在大多数浏览器中不能终止Promise链里的rejection


```
doSomething()
.then(function(result) {
  return doSomethingElse(result);
})
.then(newResult => doThirdThing(newResult))
.then(() => doFourthThing())
.catch(error => console.log(error));

```
注意：() => x 是 () => { return x; } 的简写。

上述代码的写法就是具有适当错误处理的简单明确的链式写法。