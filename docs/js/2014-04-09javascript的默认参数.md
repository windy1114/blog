#javascript的默认参数

在javascript的未来版本中，定义函数的时候可以给参数声明默认值。如下：
```js
    function myFunc(param1, param2 = "second string") {
        console.log(param1, param2);
    }

    // Outputs: "first string" and "second string"
    myFunc("first string");

    // Outputs: "first string" and "second string version 2"
    myFunc("first string", "second string version 2");
```
但是在现行的版本中这样是不行的，`Uncaught SyntaxError: Unexpected token = `。要实现上述的功能，最简单的方法有如下这种做法：

```js
    function myFunc(param1, param2) {

        if (param2 === undefined) {
            param2 = "second string";
        }

        console.log(param1, param2);
    }

    // Outputs: "first string" and "second string version 2"
    myFunc("first string", "second string version 2");
```
上述方法依赖于函数在调用时参数是缺省的。

如果我们有多个参数时，可以把它放到一个对象里面来传递。
```js
    function myFunc(paramObject) {
        var defaultParams = {
            param1: "first string",
            param2: "second string",
            param3: "third string"
        };

        var finalParams = defaultParams;

        // We iterate over each property of the paramObject
        for (var key in paramObject) {
            // If the current property wasn't inherited, proceed
            if (paramObject.hasOwnProperty(key)) {
                // If the current property is defined,
                // add it to finalParams
                if (paramObject[key] !== undefined) {
                    finalParams[key] = paramObject[key];
                }
            }
        }

        console.log(finalParams.param1,
                    finalParams.param2,
                    finalParams.param3);
    }
```

更好的方法是用jquery或者undercore的``extend`方法

```js
    function myFunc(paramObject) {
        var defaultParams = {
            param1: "first string",
            param2: "second string",
            param3: "third string"
        };

        var finalParams = _.extend(defaultParams, paramObject);

        console.log(finalParams.param1,
                    finalParams.param2,
                    finalParams.param3);
    }

    // Outputs:
    // "My own string" and "second string" and "third string"
    myFunc({param1: "My own string"});
```

##原文链接##
[http://us6.campaign-archive1.com/?u=2cc20705b76fa66ab84a6634f&id=ce9ce7921e](http://us6.campaign-archive1.com/?u=2cc20705b76fa66ab84a6634f&id=ce9ce7921e "Default Parameters in JavaScript")

##扩展阅读##
[https://brendaneich.com/2012/10/harmony-of-dreams-come-true/](https://brendaneich.com/2012/10/harmony-of-dreams-come-true/ "harmony-of-dreams-come-true")
