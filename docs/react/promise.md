const promise1 = new Promise(resolve=>setTimeout(resolve, 1000))
const promise2 = Promise.reject(200)

1.避免其中一个promise失败，promise.all进入拒绝状态
Promise.all([promise1.catch(()=>{status: "fail"}), promise2.catch(()=>{status: "fail"})]).then(()=>{
	console.log("已完成")
}).catch(()=>{console.log("已完成")})

Promise.allSetteled

只取决于当前回调函数的状态

promise是无法终止的。想办法不让promise进入已完成状态，那这个promise就是取消的状态。 可以通过设置变量来控制。

let canceld = false

const promise2 = new Promise(resolve => {
	setTimeout(()=>{
		resolve()
	} , 2000)
});

promise2.then(()=>{
	if (canceld) {
		console.log("已完成")
	}
}).catch(()=>{
	if (canceld) {

	}
})