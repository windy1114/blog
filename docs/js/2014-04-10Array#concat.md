#数组的concat方法

`concat`用于连接两个或多个数组。它不会改变原有的数组，而是返回一个新的数组。

```js
    var mhr = [];

    var heroes = ["Captain Marvel", "Aquaman"];
    var villains = ["Black Adam", "Ocean Master"];

    mhr = mhr.concat(heroes);

    // Outputs: ["Captain Marvel", "Aquaman"]
    console.log(mhr);

    mhr = mhr.concat(villains);

    // Outputs: [
    //     "Captain Marvel",
    //     "Aquaman",
    //     "Black Adam",
    //     "Ocean Master"
    // ]
    console.log(mhr);
```

上述可见，当我们传入数组，`concat`会创建一个既包含`mhr`的元素又包含传入的数组的元素的数组， 并且保留了数组的原有顺序。

`concat`可以接收多个参数，而且可以是非数组元素。

```js
    var mhr = [];

    var heroes = ["Captain Marvel", "Aquaman"];
    var villains = ["Black Adam", "Ocean Master"];

    mhr = mhr.concat(heroes, villains, "Death");

    // Outputs: [
    //     "Captain Marvel",
    //     "Aquaman",
    //     "Black Adam",
    //     "Ocean Master",
    //     "Death"
    // ]
    console.log(mhr);
```

我们可以检测一下，`concat`之后数组`mhr`是否被修改过。

```js
    var mhr = [];

    var original = mhr;

    // Outputs: true
    console.log(mhr === original);

    var heroes = ["Captain Marvel", "Aquaman"];
    //mhr.concat(heroes)生成了一个新的数组，mhr = mhr.concat(heroes), mhr指向了新生成的数组，而不再指向之前的mhr了 
    mhr = mhr.concat(heroes);

    // Outputs: false
    console.log(mhr === original);
```
上述代码中如没有将`mhr.concat(heroes)` 赋值给 `mhr` 那么 `mhr === original`是`true`



##原文##
[http://designpepper.com/blog/drips/building-up-arrays-with-array-concat.html](http://designpepper.com/blog/drips/building-up-arrays-with-array-concat.html "Building Up Arrays with Array#concat`")