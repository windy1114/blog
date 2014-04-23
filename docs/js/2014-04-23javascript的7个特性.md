#javascript的七个特性

##相等##
javascript中有两个相等运算符`==`和`===`。

```js
    var x = 1;
    if(x == "1") {
        console.log("YAY! They're equal!");
    }
```
相等运算(==)会在比较前做强制类型转换。上面的例子中，字符“1”会被转换为数字1，然后再与变量X做比较。

严格相等(===)不会强制类型转换。如果运算符两边的数据类型不一样，结果将不相等。

```js
    var x = 1;

    // with strict equality, the types must be the *same* for it to be true
    if(x === "1") {
        console.log("Sadly, I'll never write this to the console");
    }

    if(x === 1) {
        console.log("YES! Strict Equality FTW.")
    }
```
*推荐总是用严格相等(===)*

##点运算和中括号运算##


##函数上下文##

**全局作用域**
默认情况下 `this`指向全局对象。在浏览器中，指的是`window`对象 (node.js中指的是`global`).

**方法中的this**

如果一个对象有一个函数成员，调用该成员函数时，`this`指的是该对象。

```js
	var marty = {
	    firstName: "Marty",
	    lastName: "McFly",
	    timeTravel: function(year) {
	        console.log(this.firstName + " " + this.lastName + " is time traveling to " + year);
	    }
	}
	
	marty.timeTravel(1955);
	// Marty McFly is time traveling to 1955
```



##原文##
[http://developer.telerik.com/featured/seven-javascript-quirks-i-wish-id-known-about/](http://developer.telerik.com/featured/seven-javascript-quirks-i-wish-id-known-about/ "Seven JavaScript Quirks I Wish I’d Known About`")