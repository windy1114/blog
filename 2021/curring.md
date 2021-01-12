## 函数柯里化
将一个具有多个参数的函数转化成一系列带有一个参数的函数，返回一个新的函数。

函数add(a,b,c) 柯里化之后
``` 
function add(a) {
	return function(b) {
		return function(c) {
			return a+b+c
		}
	}
}
const add = num1 => num2 => num1+num2
add(num1)(num2)
```

### 函数柯里化的好处
- 参数复用
- 提前返回
- 延迟绑定

### 实现一个函数的柯里化
 接收一个需要柯里化的函数，收集该函数的参数
 返回一个新的函数，并收集参数， 在新函数里调用需要柯里化的函数，并传入所有的参数。
 并且将其结果返回




