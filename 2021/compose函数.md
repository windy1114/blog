## compose函数
把前面一个函数的执行结果，作为后一个函数的入参，一直链式的执行下去。返回一个函数
作用就是解决了函数嵌套函数的问题。

```
function compose(...fns) {
  return function(...args) {
    return fns.reducer((prevValue, currentFn) => currentFn(prevValue), ...args)
  }
}
```

