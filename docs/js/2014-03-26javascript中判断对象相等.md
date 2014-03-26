# javascript 判断对象相等

在javascript中两个对象，有一样的的属性和一样的属性值，并不代表这两个对象是相等的。比如这样的：
    var jangoFett = {
	    occupation: "Bounty Hunter",
	    genetics: "superb"
	};
	
	var bobaFett = {
	    occupation: "Bounty Hunter",
	    genetics: "superb"
	};
	
	// Outputs: false
	console.log(bobaFett === jangoFett);
    
	// Outputs: false
	console.log(bobaFett == jangoFett);


原因是javascript中数据是否相等，分为两种。一种是对简单数据类型如`string`，`number`，是通过直接比较他们的值来判断。一种是对复杂数据类型如`array`,`date`,`object`是比较它们是否为同一个引用来判断的。
比如：

    var jangoFett = {
	    occupation: "Bounty Hunter",
	    genetics: "superb"
	};
	
	var bobaFett = {
	    occupation: "Bounty Hunter",
	    genetics: "superb"
	};
	
	var callMeJango = jangoFett;
	
	// Outputs: false
	console.log(bobaFett === jangoFett);
	
	// Outputs: true
	console.log(callMeJango === jangoFett);

在上面的例子中`jangoFett`和`bobaFett`是两个具有相同属性和属性值，但是指向不同引用的对象。而`jangoFett`和`callMeJango`是两个指向同事引用的对象。

下面的方法通过遍历两个对象的属性，检测两个对象的**值**是否相等
    function isEquivalent(a, b) {
	    //取出对象的所有属性
	    var aProps = Object.getOwnPropertyNames(a);
	    var bProps = Object.getOwnPropertyNames(b);
	
	    // 如果属性的个数不一样,
	    // 那么对象不相等
	    if (aProps.length != bProps.length) {
	        return false;
	    }
	
	    for (var i = 0; i < aProps.length; i++) {
	        var propName = aProps[i];
	
	        // 如果同一个属性的值不相等,
	        // 那么对象不相等
	        if (a[propName] !== b[propName]) {
	            return false;
	        }
	    }
	
	    return true;
	}
	
	// Outputs: true
	console.log(isEquivalent(bobaFett, jangoFett));

上面的方法不适用于以下情况：

- 如果某属性的值本身又是一个对象
- 如果某属性的值是`NaN` (js中`NaN`不等于`NaN`)
- 如果对象 `a` 有一个属性值为 `undefined`，而`b`不具有这个属性。



