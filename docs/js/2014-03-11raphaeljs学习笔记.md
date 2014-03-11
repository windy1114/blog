# Raphael 学习笔记

## 简介 ##
Raphael是一个轻量级的javascript 矢量图形库。可以简单的实现图表，图片裁剪，旋转等操作。

Raphael创建的每一个图片对象都是一个dom对象，可以对它绑定事件，和修改它们。

Raphael目前支持的浏览器有：Firefox 3.0+, Safari 3.0+, Chrome 5.0+, Opera 9.5+, Internet Explorer 6.0+.


## 核心Raphael() ##
创建一个canvas对象。第一步必须要创建画布，所有绘图方法都会被绑定到这个canvas对象上。

### 参数 ###
它的参数有几种形式：

**第一种形式的参数**
<table>
	<tr>
		<td>container</td>
		<td>HTML元素 string </td>
	    <td>DOM 元素或者元素的id, 它将成为以后绘制的图形对象的父结点，即容器</td>
	</tr>
    <tr>
		<td>width</td>
		<td>number</td>
	    <td></td>
	</tr>
    <tr>
		<td>height</td>
		<td>number</td>
	    <td></td>
	</tr> 
    <tr>
		<td>callback</td>
		<td>function</td>
	    <td>回调函数, 画布创建后将触发回调函数</td>
	</tr>  
</table>

**用法：**

html代码
>``` html
> <div id="holder"></div>
>```

javascript 代码
```
    // 每个示例都创建一个画布
   // 画布的尺寸是：宽 320px 高 200px.
   // 画布开始于  左上角，并且在 id=“notepad” 的元素里面创建
   // (注意：当节点有属性： dir="rtl"，会从右上角开始创建)
   var paper = Raphael(document.getElementById("notepad"), 320, 200);
   // 同上
   var paper = Raphael("notepad", 320, 200);
   // 图像
   var set = Raphael(["notepad", 320, 200, {
     type: "rect",
     x: 10,
     y: 10,
     width: 25,
     height: 25,
     stroke: "#f00"
   }, {
     type: "text",
     x: 30,
     y: 40,
     text: "Dump"
 }]);
```

**第二种形式的参数**

<table>
	<tr>
		<td>x</td>
		<td>num</td>
	    <td>起始位置的X座标</td>
	</tr>
    <tr>
		<td>y</td>
		<td>num </td>
	    <td>起始位置的Y座标</td>
	</tr>
    <tr>
		<td>width</td>
		<td>number</td>
	    <td></td>
	</tr>
    <tr>
		<td>height</td>
		<td>number</td>
	    <td></td>
	</tr> 
    <tr>
		<td>callback</td>
		<td>function</td>
	    <td>回调函数, 画布创建后将触发回调函数</td>
	</tr>  
</table>

**用法**

javascript 代码
```
  // 创建一个画布
   // 画布的尺寸是：宽 320px 高 200px.
   // 画布开始于 10,50 .
   var paper = Raphael(10, 50, 320, 200);
   // 画布开始于  左上角，并且在 id=“notepad” 的元素里面创建
   // (注意：当节点有属性： dir="rtl"，会从右上角开始创建)
   
```

**第三种形式的参数：**

<table>
	<tr>
		<td>all</td>
		<td>array</td>
	    <td>数组的前三个值代表的是[容器ID, 宽度，高度]；或者数组的前4个值代表的是[x, y, width, height]，其余元素是描述形如{type: 类型，<属性>}</td>
	</tr>
    <tr>
		<td>callback</td>
		<td>function</td>
	    <td>回调函数, 画布创建后将触发回调函数</td>
	</tr>  
</table>

**用法**
javascript 代码
```
  // 在notepad容器里创建一个画布
   // 画布的尺寸是：宽 320px 高 200px.
   // 第一个json对象里描述的是一个矩形，第二个json对象里描述的是文字
   var set = Raphael(["notepad", 320, 200, {
     type: "rect",
     x: 10,
     y: 10,
     width: 25,
     height: 25,
     stroke: "#f00"
   }, {
     type: "text",
     x: 30,
     y: 40,
     text: "Dump"
 }]);
```


#### 例子 ####
**创建画布**
//假设有一个id为canva_container的div
    var paper = Raphael('canva_container', 500, 500);

**创建形状**

**1.画圆**
//在画布paper上画一个圆，x,y座标都是相对于paper对象而言
    var circle = paper.circle(100, 100, 80);

//创建任意多个圆,而不需要给它们指定变量

```
for(var i = 0; i < 5; i+=1) {
    var multiplier = i*5;
    paper.circle(250 + (2*multiplier), 100 + multiplier, 50 - multiplier);
}
```

**2.画矩形**
rect(x, y, width, height);

    var rectangle = paper.rect(200,200,250,100);

**3.画椭圆**
ellipse(x, y, x-radius, y-radius)

    paper.ellipse(200, 400, 100, 50);

**4.创建路径**
path([pathString])

路径字符串格式：
将鼠标移动到x = 250, y = 250，如下表示
> "M 250 250"

"M"表示我们想要把鼠标移动到画布的x,y坐标位置，而不画出来。
现在鼠标已经到了我们想要的位置，相对于这个点画一条用小写的L即"l"表示：
> "M 250 250 l 0 -50"

沿着y轴的正方向画一条50px的直线。

> "M 250 250 l 0 -50 l -50 0 l 0 -50 l -50 0 l 0 50 l -50 0 l 0 50 z"

    var tetronimo = paper.path("M 250 250 l 0 -50 l -50 0 l 0 -50 l -50 0 l 0 50 l -50 0 l 0 50 z");
画了一个俄罗斯方块
![](http://p15.qhimg.com/t01d246f787e6d30fd2.png)

"z" 表示关闭路径。它将画一条线连结由M指定的起始位置。

**5.属性样式**
attr()方法. 将对象的属性键值对作为参数。
    
	tetronimo.attr({fill: '#9cf', stroke: '#ddd', 'stroke-width': 5});

**一个单词可以不用引号括号，两个单词如stroke-width需要用引号括起来**

**6.动画**

**7.访问dom**
通过节点属性，可以很方便的绑定事件到形状上。
例子：
```javascript
   //创建一个画布
   var paper = new Raphael(document.getElementById('canvas_container'), 500, 500);
   //在画布上画一个圆 
   var circ = paper.circle(250, 250, 40);
   circ.attr({fill: '#000', stroke: 'none'});
   //在画布上创建文字，透明度为0文字最终将不显示，toBack()方法将文字置于所以形状的下面，toFront()置于最前面
   var text = paper.text(250, 250, 'Bye Bye Circle!')
   text.attr({opacity: 0, 'font-size': 12}).toBack();
   //给圆的node绑定click方法
   circ.node.onclick = function() {
      this.style.cursor = 'pointer'
      text.animate({opacity: 1}, 2000);
      circ.animate({opacity: 0}, 2000, function() {
         this.remove();
     });
   }
```
