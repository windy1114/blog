loader是一个函数， 它不能是箭头函数，因为需要用this去调用loader api,
返回字符串？

1. 配置webpack.config.js
```
//告知webpack 如果node_modules里面找不到就是customerLoaders（自定义loader 存放的地方）去查找
resolveLoader: {
    modules: [
      "./node_modules", 
      "./customerLoaders"
    ]
  }
  
```
2. 配置文件编译的规则

```
    rules:[{
      test: /.less$/,
      use: [
        'styleLoader', 'cssLoader', 'lessLoader'
      ]
    }]
```

### 编写一个处理less文件的loader

#### lessLoader.js
```
const less = require("less");
module.exports = function (source) {
  less.render(source, (e, output) => {
    this.callback(e, output.css);
  });
};

```
#### cssLoader.js
```
module.exports = function (source) {
  console.log(source);
  return JSON.stringify(source);
};

```
#### styleLoader.js
```
module.exports = function (source) {
    console.log(source);

  return `const ele = document.createElement('style');
    ele.innerHTML = ${source};
    document.head.appendChild(ele);
  `;
 
};
```

