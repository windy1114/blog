# 鼠标事件

##鼠标事件类型##

###简单事件###

**mousedown**
按下鼠标任意按钮时触发

**mouseup**
释放鼠标时触发

**mouseover**
鼠标经过元素时触发

**mouseout**
鼠标离开元素时触发

**mousemove**
鼠标在元素内部移动时重复的触发

###复杂事件###

**click**
鼠标点击时，或者按下回车键时触发

**contextmenu**
鼠标右击时触发

**dblclick**
鼠标双击时触发

## 事件触发顺序 ##
一个简单的行为可能会引起多种事件。比如，一次单击会顺次触发 `mousedown`, `mouseup`和`click`事件。

`mousedown`,`mouseup`,`click`, `dblclick` 这4个事件的触发顺序始终如下：
1. mousedown
2. mouseup
3. click
4. mousedown
5. mouseup
6. click
7. dblckick

但是IE9以下版本浏览器，在双击事件中会跳过第二个`mousedown`, `click`事件，其顺序如下：
1. mousedown
2. mouseup
3. click
4. mouseup
5. dblclick

> 如何处理`click`和`dblclick`事件冲突
> 一次双击事件会触发两次`click`和一次`dblclick`事件，我们可以通过设置时间间隔来避免不必要的`click`事件监听。

    var timer1 = null,
        timer2 = null;

        function setColor(color) {
            $('.el2').css('background', color);
            console.log("color", color);
        }

        $(document).on('click', function () {
            clearTimeout(timer1);
            clearTimeout(timer2);
            timer1 = setTimeout(function () {
                setColor('#f00');
                console.log('click ' + (new Date()).getTime());
            }, 300);
           
        });

         $(document).on('dblclick', function () {
            clearTimeout(timer1);
            clearTimeout(timer2);
            timer1 = setTimeout(function () {
                setColor('#ff0');
                console.log('dblclick ' + (new Date()).getTime());
            }, 300);
            
        });


## mouseove/mouseout 和relatedTarget ##
当鼠标从一个元素移动到另一个元素时，这两个元素是互相关联的。两个元素都有相应的事件属性。
**mouseover**
获得光标的元素是 `event.target`(IE: `scrElement`)
失去光标的元素是 `event.relatedTarget` (IE: `fromElement`)

**mouseout**
失去光标的元素是 `event.target`(IE: `scrElement`)
获得光标的元素是 `event.relatedTarget` (IE: `toElement`)

    // from IE to W3C
	if (e.relatedTarget === undefined) {
	  e.relatedTarget = e.fromElement || e.toElement
	}

**event.relatedTarget(IE类似)可以为 null**
当鼠标从窗口之外经过元素时 `mouseover`的`relatedTarget = null`
当鼠标从元素上移到窗口之外时 `mouseout` 的 `relatedTarget = null`

## 频繁的mousemove和mouseover ##
每一次鼠标移动都会触发 `mousemove` 事件。它的目标元素是鼠标经过中嵌套层次最深的元素。

> Mousemove and mouseover/mouseout trigger when browser internal timing allows.
> That means if you move the mouse fast, intermediate DOM elements and parents are be skipped.

So you can move over an element without any mousemove/mouseover triggered on it.

You can move from a child through parent without any mouse event on the parent.

Although browser can skip intermediate elements, it guarantees that as far as mouseover was triggered, the mouseout will trigger too.

## mouseenter和mouseleave ##



## 鼠标坐标 clientX(Y), pageX(Y) ##

**相对于window**
----------
`clientX/clientY` 鼠标相对于窗口所在的坐标。 所以浏览器都支持这两个属性。
如果窗口的大小是500X500, 鼠标在正中间，那么`clientX`和`clientY`都等于250.
此时如果滚动滚动条，`clientX`和`clientY`的值都不会发生变化。

**相对于document**
----------
`pageX/pageY `  鼠标相对于文档所在的坐标。
如果窗口的大小是500X500, 鼠标在正中间，那么`pageX`和`pageY`都等于250.
此时如果滚动滚动条往下拉250，`pageY`的值就变为500.

IE9以下浏览器不支持这两个属性

IE9以下浏览器中可以通过将页面的滚动值添加到`clientX/clientY`中来获得`pageX/pageY`的值。

在标准文档模式下scroll的值在html元素上，即`document.documentElement.scrollLeft`, 在quirks模式下，它的值在body上，即`document.body.scollLeft`。在quirks模式下，如果body没有加载完scroll的值就会为0。

    var html = document.documentElement,
		body = document.body,
		e.pageX = e.clientX + (html.scrollLeft || body && body.scrollLeft || 0);



## 阻止选中 ##
在文字上的点击鼠标操作，一个常见的问题就是会把文字选中，比如双击一个单词。要阻止选中，我们需要阻止浏览器的默认行为 IE浏览器的`selectstart`, 其他浏览器的`mousedown`事件。

``` html
	<span 
  		ondblclick="alert('ok')"
  		onselectstart="return false"
  		onmousedown="return false"
	>Text</span>

```