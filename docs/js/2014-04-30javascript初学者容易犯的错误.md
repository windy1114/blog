#javascript初学者容易犯的错误#

##忘记{}##
总是容易忘记`if, else, while and for`语句后面的花括号，尽管这是被允许的，但是你需要格外的小心，因为这样有可能得不到你想要的结果。

```js
    // Say hello to Gandalf
    hello('Gandalf');

    function hello(name){

        // This code doesn't do what the indentation implies!

        if(name === undefined)
            console.log('Please enter a username!');
            fail();

        // The following line is never reached:

        success(name);
    }

    function success(name){
        console.log('Hello, ' + name + '!');
    }

    function fail(){
        throw new Error("Name is missing. Can't say hello!");
    }
```
虽然表面上，`fail`函数像是在`if`语句里面才执行，实际不是这样，而是总是会被执行的。所以最好是用花括号把语句块括起来，尽管自由一条语句。


##忘记分号##



##不理解类型强制转换##

js是弱类型语言，在定义变量的时候不需要声明数据类型。
```js
    // Listen for the input event on the textbox

    var textBox = document.querySelector('input');

    textBox.addEventListener('input', function(){

        // textBox.value holds a string. Adding 10 appends 
        // the string '10', it doesn't perform an addition..

        console.log(textBox.value + ' + 10 = ' + (textBox.value + 10));

    });
```


##忘记var##
定义变量的时候容易忘记`var`，会被默认为定义了一个全局变量。
```js
    var a = 1, b = 2, c = 3;

    function alphabet(str){
        var a = 'A', b = 'B'    // Oops, missing ',' here!
            c = 'C', d = 'D';

        return str + ' ' + a + b + c + '…';
    }

    console.log( alphabet("Let's say the alphabet!") );

    // Oh no! Something went wrong! c has a new value!
    console.log(a, b, c);

    //->logs: Let's say the alphabet! ABC…
    //->logs: 1 2 C
```
上面的代码中，当编译器到达第四行的时候，会自动插入`;`(分号)，这样就导致了c,d为全局变量，从而改变了强局变量c的值。了解更多[https://github.com/stevekwan/best-practices/blob/master/javascript/gotchas.md](https://github.com/stevekwan/best-practices/blob/master/javascript/gotchas.md "javascript gotchas")


##对浮点数进行运算##
原因是[http://stackoverflow.com/questions/588004/is-floating-point-math-broken](http://stackoverflow.com/questions/588004/is-floating-point-math-broken "浮点数在内存中表示")

```js
    var a = 0.1, b = 0.2;

    // Surprise! this is false:
    console.log(a + b == 0.3);

    // Because 0.1 + 0.2 does not produce the number that you expect:
    console.log('0.1 + 0.2 = ', a + b);
```
如果一定要使用精确的小数运算，可以用[https://github.com/MikeMcl/bignumber.js](https://github.com/MikeMcl/bignumber.js "bignumber.js")来解决这个问题。


##不使用字面量的方式定义数组、字符串、对象##
不推荐使用 new Array(), new Object(), new String()的方式

```js
    /* Using array constructors is valid, but not recommended. Here is why. */

    // Create an array with four elements:

    var elem4 = new Array(1,2,3,4);
    //logs: 4
    console.log('Four element array: ' + elem4.length);

    // Create an array with one element. It doesn't do what you think it does:

    var elem1 = new Array(23);
    //logs: 23
    console.log('One element array? ' + elem1.length);

    /* String objects also have their warts */

    var str1 = new String('JavaScript'),
        str2 = "JavaScript";

    // Strict equality breaks:
    // logs: false
    console.log("Is str1 the same as str2?", str1 === str2);
```

##不理解作用域##

```js
    // Print the numbers from 1 to 10, 100ms apart. Or not.

    for(var i = 0; i < 10; i++){
        setTimeout(function(){
            console.log(i+1);
        }, 100*i);
    }

    /* To fix the bug, wrap the code in a self-executing function expression:

    for(var i = 0; i < 10; i++){

        (function(i){
            setTimeout(function(){
                console.log(i+1);
            }, 100*i);
        })(i);

    }               

    */
```
这里我们用了`setTimeout`来延迟执行,当它执行的时候，循环其实已经执行完了，`i`的值已经变为11.

在外面加上一个自执行函数，它把'i'的值copy了一份，并且是每个timeout函数的私有copy。更多关于函数作用域[https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions_and_function_scope](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions_and_function_scope "Functions and function scope")


##使用eval##
[http://jslinterrors.com/eval-is-evil/](http://jslinterrors.com/eval-is-evil/ 'Eval is evil') 当你使用eval运算时，通常有更好更快的方法。

```js
    /* Using eval to access properties dynamically */

    var obj = {
        name: 'Foo Barski',
        age: 30,
        profession: 'Programmer'
    };

    // Which property to access?
    var access = 'profession';

    // This is a bad practice. Please don't do it:
    console.log( eval('obj.name + " is a " + obj.' + access) );

    // Instead, use array notation to access properties dynamically:
    console.log( obj.name + " is a " + obj[access]);

    /* Using eval in setTimout */

    // Also bad practice. It is slow and difficult to read and debug:
    setTimeout(' if(obj.age == 30) console.log("This is eval-ed code, " + obj[access] + "!");', 100);

    // This is better:
    setTimeout(function(){

        if(obj.age == 30){
            console.log('This code is not eval-ed, ' + obj[access] + '!');
        }

    }, 100);
```
`eval`里的参数是字符型，调试eval语句块是难以理解的，有时还需要混合使用单双引号。更别提它比正常的js要慢了。



##不理解异步##
javascript的一个特点是，几乎所有的事情都是异步的， 我们需要通过传递回调函数来通知事件。

```js
    var userData = {};

    // Fetch the location data for the current user.
    load();

    // Output the location of the user. Oops, it doesn't work! Why?
    // userData.ip -> undefined 
    // userData.country_name -> undefined
    console.log('Hello! Your IP address is ' + userData.ip + ' and your country is ' + userData.country_name);

    // The load function will detect the current visitor's ip and location with ajax, using the
    // freegeoip service. It will place the returned data in the userData variable when it's ready.

    function load(){

        $.getJSON('http://freegeoip.net/json/?callback=?', function(response){
            userData = response;

            // Uncomment this line to see what is returned:
            console.log(response);
        });
    }
```
虽然`console.log` 在`load()`函数调用后执行，但实际上它在数据返回以前就已经执行了。

##事件监听使用不当##



##原文##
[http://tutorialzine.com/2014/04/10-mistakes-javascript-beginners-make/](http://tutorialzine.com/2014/04/10-mistakes-javascript-beginners-make/"10 Mistakes That JavaScript Beginners Often Make`")