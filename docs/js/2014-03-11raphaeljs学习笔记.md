# Raphael 学习笔记 #

## 简介 ##
Raphael是一个轻量级的javascript 矢量图形库。可以简单的实现图表，图片裁剪，旋转等操作。

Raphael创建的每一个图片对象都是一个dom对象，可以对它绑定事件，和修改它们。

Raphael目前支持的浏览器有：Firefox 3.0+, Safari 3.0+, Chrome 5.0+, Opera 9.5+, Internet Explorer 6.0+.


## 核心Raphael() ##
创建一个canvas对象。第一步必须要创建画布，这个实例将来调用的所有绘图方法都会被绑定到这个canvas对象上。

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
> ```
>  // 每个示例都创建一个画布
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
> ```

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
> ```
>  // 创建一个画布
   // 画布的尺寸是：宽 320px 高 200px.
   // 画布开始于 10,50 .
   var paper = Raphael(10, 50, 320, 200);
   // 画布开始于  左上角，并且在 id=“notepad” 的元素里面创建
   // (注意：当节点有属性： dir="rtl"，会从右上角开始创建)
   
> ```

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
> ```
>  // 在notepad容器里创建一个画布
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
> ```