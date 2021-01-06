## Promise
### Promise是什么
异步编程的一种解决方案。
Promise是一个构造函数，接收一个函数(excutor执行器)作为参数。
### Promise解决了什么问题
Promise通过延迟回调函数绑定，异常穿透，返回值传透解决了回调地狱问题。
### Promise的基本用法
```
	new Promise(function(resolve, reject) => {
		//待处理的异步操作
		//异步任务顺利完成，调用resolve;异步任务失败调用reject
	})
```
想让一个函数拥有promise功能，只需要让它返回一个promise即可。
```
	function xhrPromise(url) {
		return new Promise((resolve, reject)=>{
			const xhr = new XMLHTTPRequest()
			xhr.open("GET",url)
			xhr.onload = () => resolve(xhr.responseText)
			xhr.onerror = () => reject(xhr.statusText)
			xhr.send()
	}
```
### Promise的链式调用
- Promies.prototype.then
- Promise.prototype.catch

### Promise的常用方法
- Promise.resolve()
- Promise.reject()
- Promise.all()
- Promise.race()

### 怎么改变Promise的状态
1. resove(value) promise的状态由pedding变为fullfilled
2. reject(reason) promise的状态由pedding变为rejected
3. throw  promise的状态由pedding变为rejected

### 如何中断Promise
在回调函数中返回一个pendding状态的promise对象
### Promise的基础实现

```
	function Promise(excutor) {
		const _this = this
		_this.status = "pedding"
		_this.data = null
		_this.onFullfilled = null
		_this.onRejected = null

		const resolve = value => {
			
			if(_this.status === "pedding") {
				return;
			}

			_this.status = "fullfilled"

			return value;

		}

		const reject = reason => {
			if(_this.status === "pedding") {
				return;
			}

			_this.status = "rejected"
			return reason;
		}

		try {
			excutor(resolve, reject)
		} 
		catch (error) {
			throw(error)
		}
	}
	Promise.prototype.then = function (onResolved, onRejected) {

	}
```



