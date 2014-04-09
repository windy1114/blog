#querySelectorAll

Jquery最具特色的特点就是用CSS选择器来选择相应的操作元素。
如下html:

```html
    <!doctype html>
    <html>
        <head>
            <title>Programming Languages</title>
        </head>
        <body>
            <h1>Programming Languages</h1>

            <ul>
                <li>JavaScript</li>
                <li>CoffeeScript</li>
                <li>Ruby</li>
                <li>Python</li>
            </ul>

            <script src="example.js"></script>
        </body>
    </html>
```
如果我们要通过jquery来对第一个和最后一个li添加一个样式。可以这样做到：
```js
    $("li:first-child, li:last-child").addClass("list-end");
```
但是在现代浏览器中，有一个原生的DOM方法可以用CSS 选择器来实现同样的功能，如：

```js
    var selector = "li:first-child, li:last-child";
    var listEnds = document.querySelectorAll(selector);
    var listEndsArr = [].slice.call(listEnds);

    listEndsArr.forEach( function (el) {
        el.className += "list-end";
    });
```

`querySelectorAll`将返回匹配的元素，这些元素是一个`nodeList`, 看起来数组对象，但不是真正的数组`array`,这就意味着，我们不能使用数组的一些方法，比如`forEach`。为解决这个问题，我们用`slice.call`方法创建一个真正的匹配元素数组，然后再用`forEach`方法来实现。

值得注意的两点：

- 所用的css选择器必须是浏览器所支持的；
- IE7及以下浏览器不支持`querySelectorAll`；

##原文##
[http://designpepper.com/blog/drips/ditching-jquery-with-queryselectorall.html](http://designpepper.com/blog/drips/ditching-jquery-with-queryselectorall.html "Ditching jQuery with `querySelectorAll`")