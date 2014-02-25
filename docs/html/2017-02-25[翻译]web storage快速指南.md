# [翻译]Web Storager快还指南 

几乎所有的桌面应用或移动应用都需要存储用户的数据。但是对于网站呢？在过去，我们用cookies来存储，但是它有严重的局限性。HTML5给了我们更好的工具来解决这个问题。一个是IndexedDB，一个是Web Storager。

## 什么是Web Storage ##
通俗的说 Web Storage 就是通过一系列的api来提供一种将客户端数据存储到浏览器的简单方法。它比cookie更加的安全、快速。跟cookie一样，数据存储在用户的浏览器不会在传输到网络上。它可以存储的数据比cookie大得多，而不会影响网站的性能。

Web Storage提供两个优秀的对象来存储数据：<br>
- **localStorage:** 你存储的数据不需要设置过期日期，这意味着你存储的数据永不过期。
- **sessionStorage:** 当用户关闭浏览器窗口（非标签页tab），数据才会过期。适用于存储临时数据，比如用户填写的表单数据，以免因意外关闭浏览器的标签页（tab）或者是刷新页面而导致的数据丢失。

## 开始使用 ##

### localStorage ###
将数据存储到localStorage非常的简单，只需要设置一个属性。读取的数据也同样简单。例子：

> ```javascript
> localStorage.myText = 'This is some text which have been stored with localStorage object';
document.getElementById("text").innerHTML= localStorage.myText;
> ```

###sessionStorage###
通过下面的方法就可以读取和存储sessionStorage的数据
> ```javascript
> sessionStorage.myText = 'This is some text which have been stored with sessionStorage object';
document.getElementById("text").innerHTML= sessionStorage.myText;
> ```
 
两个对象都有`setItem()`, `getItem()` 和 `removeItem()`方法。
> ```javascript
> localStorage.setItem('username','Johnny');
console.log(localStorage.getItem('username'));
localStorage.removeItem('username'); // Johnny is no more!
> ```

也可像普通对像一样，对他们运算，比如计算它的长度：
> ```javascript
> console.log(localStorage.length);
for(var i in localStorage){ console.log(localStorage[i]);}
> ```
  
 
## 浏览器支持性 ##
在使用它之前需要检测浏览器是否支持。Web Storage已被所有的现代浏览器支持包括IE8以上。IE7及以下IE浏览器不支持，如果想要支持它，需要使用下面的向后兼容方法。


## 支持Web Storage 的javascript 库##

- [http://addyosmani.github.io/basket.js/](http://addyosmani.github.io/basket.js/ "basket.js")
- [https://github.com/ded/Kizzy](https://github.com/ded/Kizzy "Kizzy")
- [http://agnostic.github.io/LocalDB.js/](http://agnostic.github.io/LocalDB.js/ "LocalDB")
- [http://layzie.github.io/rockstage_js/](http://layzie.github.io/rockstage_js/ "Rockstage.js")
- [https://github.com/marcuswestin/store.js](https://github.com/marcuswestin/store.js "store.js")


原文链接[http://tutorialzine.com/2013/11/a-quick-guide-to-web-storage/](http://tutorialzine.com/2013/11/a-quick-guide-to-web-storage/)
  

 
 