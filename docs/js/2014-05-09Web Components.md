#Web Components
http://css-tricks.com/modular-future-web-components/
##什么是Web Components##

简单的说就是，允许我们把标签和样式绑定到普通的html元素上。这些新的元素封装了所有他们需要的html和css.

##Shadow DOM##

###创建Shadow DOM###

选择一个元素，调用它的`createShaowRoot` 方法。它会返回一个文档片段,然后我们可以在这个文档片段中填充内容。

```
  <div class="container"></div>

  <script>
    var host = document.querySelector('.container');
    var root = host.createShadowRoot();
    root.innerHTML = '<p>How <em>you</em> doin?</p>'
  </script>
```

###Shadow Host###
在shadow Dom中，调用`createShaowRoot`方法的元素就是shadow host. 是用户唯一可见的元素。如下：`video`就是shadow host.它的内容就是里面的`source`标签。

```
  <video>
    <source src="trailer.mp4" type="video/mp4">
    <source src="trailer.webm" type="video/webm">
    <source src="trailer.ogv" type="video/ogg">
  </video>
```

###Shadow Root###
`createShadowRoot` 返回文档片段，也就是`Shadow Root`. Shadow root和它的子元素对用户来说是隐藏的，但是浏览器会渲染它们。

###Shadow Boundary###

