#jQuery Coding Standars and Best Practices

## jQuery 变量 ##

- 所有用来存储jq对象的变量，其命名最好加前缀`$`
- 总是保存jq选择器的变量，以重复使用
  ```js
	 var $myDiv = $("#myDiv");
	 $myDiv.click(function(){...});
  ```
- 以驼峰方式命名变量


## 选择器 ##

- 尽可能的使用id选择器，它比较快因为是直接用`document.getElmentById()`实现的。
- 使用class选择器时，不要加上标签的类型. [性能比较](http://jsperf.com/jqeury-selector-test)
  ```js
	var $products = $("div.products"); // SLOW
	var $products = $(".products"); // FAST
  ```
- 用`find()`方法来查找ID下的子元素。
  ```js
     // BAD, a nested query for Sizzle selector engine
	var $productIds = $("#products div.id");
	
	// GOOD, #products is already selected by document.getElementById() so only div.id needs to go through Sizzle selector engine
	var $productIds = $("#products").find("div.id");
  ```
- 最右边的选择器尽量具体化，左边的选择器尽量简单 （最右边的选择器尽可能使用`tag.class`，而左边只需要`tag`或者`.class`即可）
  ```js
    // Unoptimized
	$("div.data .gonzalez");
	
	// Optimized
	$(".data td.gonzalez");
  ```
- 避免过分详细的选择器 [性能比较](http://jsperf.com/avoid-excessive-specificity)
  ```js
	$(".data table.attendees td.gonzalez");
 
	// Better: Drop the middle if possible.
	$(".data td.gonzalez");
  ```
- 避免全局选择器 [更多阅读](http://learn.jquery.com/performance/optimize-selectors/)
  ```js
    $( ".buttons > *" ); // Extremely expensive.
	$( ".buttons" ).children(); // Much better.
	 
	$( ".category :radio" ); // Implied universal selection.
	$( ".category *:radio" ); // Same thing, explicit now.
	$( ".category input:radio" ); // Much better.
  ```
- 不要对ID选择器使用层级关系
  ```js
	$('#outer #inner'); // BAD
	$('div#inner'); // BAD
	$('.outer-container #inner'); // BAD
	$('#inner'); // GOOD, only calls document.getElementById()
  ```


## DOM 操作 ##

- 操作一个已经存在的元素之前，先对它进行`detach()`,操作完之后再把它添加回去。[更多阅读](http://learn.jquery.com/performance/detach-elements-before-work-with-them/)
	```js
		var $table = $( "#myTable" );
		var $parent = $table.parent();
		 
		$table.detach();
		 
		// ... add lots and lots of rows to table
		 
		$parent.append( $table );
    ```

- 使用字符串连接或者`array.join()`的方法来代替过多的`append()`。 [更多阅读](http://learn.jquery.com/performance/append-outside-loop/)
  ```js
	// BAD
	var $myList = $("#list");
	for(var i = 0; i < 10000; i++){
	    $myList.append("<li>"+i+"</li>");
	}
	 
	// GOOD
	var $myList = $("#list");
	var list = "";
	for(var i = 0; i < 10000; i++){
	    list += "<li>"+i+"</li>";
	}
	$myList.html(list);
	 
	// EVEN FASTER
	var array = []; 
	for(var i = 0; i < 10000; i++){
	    array[i] = "<li>"+i+"</li>"; 
	}
	$myList.html(array.join(''));
 ```
- 不要操作空的元素 [更多阅读](http://learn.jquery.com/performance/dont-act-on-absent-elements/)
	```js
		// BAD: This runs three functions before it realizes there's nothing in the selection
		$("#nosuchthing").slideUp();
		 
		// GOOD
		var $mySelection = $("#nosuchthing");
		if ($mySelection.length) {
		    $mySelection.slideUp();
		}
	```

## 事件 ##
- 一个页面只用一个document ready 处理函数，这样更容易debug
- 不要对事件绑定匿名函数。匿名函数难以debug,维护，测试，重用。[更多阅读](http://learn.jquery.com/code-organization/beware-anonymous-functions/)
	```js
		// BAD
		$( document ).ready(function() {
		 
		    $( "#magic" ).click(function( event ) {
		        $( "#yayeffects" ).slideUp(function() {
		            // ...
		        });
		    });
		 
		    $( "#happiness" ).load( url + " #unicorns", function() {
		        // ...
		    });
		 
		});
		 
		// BETTER
		var PI = {
		 
		    onReady: function() {
		        $( "#magic" ).click( PI.candyMtn );
		        $( "#happiness" ).load( PI.url + " #unicorns", PI.unicornCb );
		    },
		 
		    candyMtn: function( event ) {
		        $( "#yayeffects" ).slideUp( PI.slideCb );
		    },
		 
		    slideCb: function() { ... },
		 
		    unicornCb: function() { ... }
		 
		};
		 
		$( document ).ready( PI.onReady );
	```

- 尽可能的给事件添加命名空间，这样更能容易解绑事件。
    $("#myLink").on("click.mySpecialClick", myEventHandler); // GOOD
	// Later on, it's easier to unbind just your click event
	$("#myLink").unbind("click.mySpecialClick");

## Ajax ##
- 使用`$.ajax()`方法，避免使用`$.get()`、`$.getJson()`
- 不要把参数带在url上，通过data对象来设置它们
	// Less readable...
	$.ajax({
	    url: "something.php?param1=test1&param2=test2",
	    ....
	});
	 
	// More readable...
	$.ajax({
	    url: "something.php",
	    data: { param1: test1, param2: test2 }
	});
- 对于ajax加载的内容节点对它们绑定事件，使用代理。[更多阅读](http://api.jquery.com/on/#direct-and-delegated-events)
- 使用Promise接口 [更多阅读](http://www.htmlgoodies.com/beyond/javascript/making-promises-with-jquery-deferred.html)
    $.ajax({ ... }).then(successHandler, failureHandler);
 
	// OR
	var jqxhr = $.ajax({ ... });
	jqxhr.done(successHandler);
	jqxhr.fail(failureHandler);	


## 链式写法 ##
- 使用链式写法去替代用变量缓存和多次调用变量。
	$("#myDiv").addClass("error").show(); 

## 其他 ##
- 用对象作为参数
    $myLink.attr("href", "#").attr("title", "my link").attr("rel", "external"); // BAD, 3 calls to attr()
	// GOOD, only 1 call to attr()
	$myLink.attr({
	    href: "#",
	    title: "my link",
	    rel: "external"
	});

- 不要使用不提倡的方法。经常关注每个版本的不提倡方法[http://api.jquery.com/category/deprecated/](http://api.jquery.com/category/deprecated/)
	$("#parent-container").on("click", "a", delegatedClickHandlerForAjax);




[原文地址](http://lab.abhinayrathore.com/jquery-standards/)