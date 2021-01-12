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
		//异步任务顺利完成且返回值，调用resolve;异步任务失败调用且返回值reject
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
		_this. status = "pedding"
		_this.promiseResult = null
		_this.callbacks = [] //保存then方法传入的回调函数，因为then可以链式调用，所以这里用数组

		const resolve = value => {
			//1.修改promise的状态为fullfilled
			//2.设置promise的结果值
			
			//确保promise的状态只能改变一次
			if(_this.status !== "pedding") {
				return;
			}

			_this.status = "fullfilled"
			this.data = value
			return value;

		}

		const reject = reason => {
			//1.修改promise的状态为rejected
			//2.设置promise的结果值

			//确保promise的状态只能改变一次
			if(_this.status !== "pedding") {
				return;
			}

			_this.status = "rejected"
			this.data = reason
			return reason;
		}

		//这里用try catch是因为throw可以改变promise的状态。
		try {
			//同步调用执行器函数
			excutor(resolve, reject)
		} 
		catch (error) {
			//修改promise对象的状态为失败const FULLFILLED = "fullfilled"
const REJECTED = "rejected"
const PEDDING = "pedding"
function Promise(excutor) {
  const _this = this

  _this.promiseStatus = PEDDING
  _this.promiseResult = null
  _this.callbacks = [] //保存then方法传入的回调函数，因为then可以链式调用，所以这里用数组
  const resolve = value => {
    //1.修改promise的状态为fullfilled
    //2.设置promise的结果值
    //3.执行then方法传入的onResolved 

    //确保promise的状态只能改变一次
    if (_this.promiseStatus !== PEDDING) {
      return;
    }
    _this.promiseStatus = FULLFILLED
    this.promiseResult = value
    _this.callbacks.forEach(callback => callback.onResolved(value))

  }

  const reject = reason => {
    //1.修改promise的状态为rejected
    //2.设置promise的结果值
    //3.执行then方法传入的onRejected
    //确保promise的状态只能改变一次
    if (_this.promiseStatus !== PEDDING) {
      return;
    }
    _this.promiseStatus = REJECTED
    this.promiseResult = reason
    _this.callbacks.forEach(callback => callback.onRejected(reason))

  }

  //这里用try catch是因为throw可以改变promise的状态。
  try {
    //同步调用执行器函数
    excutor(resolve, reject)
  }
  catch (error) {
    //修改promise对象的状态为失败
    reject(error)
  }
}
Promise.prototype.then = function (onResolved, onRejected) {
  //返回一个promise 这个promise的状态取决于回调函数的结果
  //根据promise的状态，执行onResolved或者onRejected,并且需要将promiseResult作为实参传递。
  //promise pedding状态需要保存回调函数

  const _this = this

  return new Promise((resolve, reject) => {
    if (_this.promiseStatus === FULLFILLED) {
      try {
        let result = onResolved(_this.promiseResult)
        //result可能是promise， 也可能是undefined或者一个值
        if (result instanceof Promise) {
          //获取该promise的返回值
          result.then((value => {
            resolve(value) //整个promise的结果
          }, reason => {
            reject(reason) //整个promise的结果
          }))
        } else {
          //then的结果为成功
          resolve(result)
        }
      } catch (error) {
        reject(error)
      }

    } else if (_this.promiseStatus === REJECTED) {
      onRejected(_this.promiseResult)
    } else {
      //保存回调(因为状态不确定，所以不能直接去执行then的回调函数)
      _this.callbacks.push({
        onResolved: function () {
          //执行成功的回调函数
          let result = onResolved(_this.promiseStatus) //根据它的执行结果会修改Promise的状态
          if (result instanceof Promise) {
            result.then(v => resolve(v), r => reject(r))
          } else {
            resolve(result)
          }
        }
        onRejected: function () {
          try { 
            let result = onRejected(_this.promiseStatus) //根据它的执行结果会修改Promise的状态
            if (result instanceof Promise) {
              result.then(v => resolve(v), r => reject(r))
            } else {
              reject(result)
            }
          } catch () {

           }
        }
      })
    }
  })

}
			reject(error)
		}
	}
	Promise.prototype.then = function (onResolved, onRejected) {
		//根据promise的状态，执行onResolved或者onRejected,并且需要将promiseResult作为实参传递。
		//promise pedding状态需要保存回调函数
	}
```



