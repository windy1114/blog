## Cookie, localStorage, sessionStorage的区别

|          | Cookies       | localStorage                                       | sessionStorage |
| -------- | ------------- | --------------------------------------------------- | -------- |
| 存储大小 |  约5KB         |  约 10MB                                            |  约5MB        |
| 过期时间 |  手动设置       | 永不过期                                            |  浏览器tab关闭 |
| 访问范围 |  不同tab可共享  | 不同tab可共享                                       | 当前 tab |
| 存储地方 |  浏览器和服务器 | 浏览器                                              | 浏览器 |
| 是否被携带 | 会           | 不会                                                | 不会 |
| 是否特定于页面协议 |       | 是 | 是 |


## Cookie
- Cookie 是什么
   服务器向浏览器发送web页面时，把一些小块数据临时写入用户计算机内，并在下次用户请求服务器的时候携带发送回服务器。

- Cookie的用途
	- 会话状态管理（用户登录状态，购物车等）
	- 个性化设置 （主题，过去没有更好的本地存储方式的时候）
	- 浏览器行为跟踪 

- Cookie的设置
	- 服务端通过Set-Cookie在响应头里面添加`Set-Cookie`选项。
	- 客户端（不能创建HttpOnly标志，也不能访问HttpOnly标志的Cookie）
	```
		document.cookie = "xxxxx=xxxx"
		console.log(document.cookie)
	```

- Cookie的生命周期
	会话期`Cookie`和持久性`Cookie`
	- 会话期Cookie， 会话结束自动删除
	- 持久性Cookie, 通过设置Expires 或者 Max-Age (过期时间，只和客户端相关，而不是服务器)
	```
		Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
	``` 

- Cookie的特性
  - 浏览器对服务器发起的每一次新的请求，都会携带之前保存过的Cookie信息给服务器（所以这会带来一定的性能消耗）
  ```
  	GET /sample_page.html HTTP/1.1
	Host: www.example.org
	Cookie: xxx_cookie=xxxx; xxx_cookie=xxxxxx
  ```
  - Domain指定了哪些主机可以接收Cookie。默认为origin,不包含子域名。
  - Path 指定了主机下哪些路径可以接受Cookie
  - SameSite 允许服务器要求某个Cookie在跨站请求时不会被发送，从而可以阻止CSRF (SameSite )





