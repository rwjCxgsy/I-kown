# webpack 基本配置

[Webpack](https://webpack.js.org/) 是目前最为流行的前端构建工具。同时在前端工程化中，Webpack在开发/编译/构建中都起到了最关键的作用。所以在当下阶段，webpack的基本配置，是每一个前端程序员应当掌握的基本技能。

## 基本配置

### 入口文件（entry）

一个webpack工程只少包含一个入口文件

```javascript
module.exports = {
  entry: './index.js'
};
```

多个入口文件

```javascript
module.exports = {
  entry: {
    pageA: './pageA.js',
    pageB: './pageB.js'
  }
};
```

也可以是一个数组

```javascript
module.exports = {
  entry: ['./fileA.js', './fileB.js']
};
// 也可以这样：
module.exports = {
  entry: {
    main: ['./fileA.js', './fileB.js']
  }
};
```

### 出口（output）

即是入口文件编译后的（出口文件）

```javascript
module.exports = {
  output: {
    filename: 'output.js', // 文件名
    path: __dirname + '/dist' // 文件输出路径，必须为系统绝对路径
  }
};
```

如果有多个入口文件，就会有多个出口文件

```javascript
module.exports = {
  entry: {
    pageA: './a.js',
    pageB: './b.js'
  },
  output: {
    filename: '[name].js', // 这里的name表示pageA，pageB
    path: __dirname + '/dist'
  }
};

// 编译后
/*
    dist/pageA.js
    dist/pageB.js
*/
```

#### path和publicPath 区别

==path== 配置决定了最终打包后输出资源的文件路径，且它必须要求为系统绝对路径
==pubclic== 表示

### mode （webpack4新增）

### 模块（module）

我们知道webpack 是一个模块打包器，它不仅仅能处理js文件，还能处理css、图片。而且也能将ES6的代码、甚至是TypeScript的代码引用并打包输出成当下可执行的js文件。而webpack自身不可能穷举处理所有的相关文件。于是就采用了 loader 方式。

> loader 用于对模块的源代码进行转换。loader 可以使你在 import 或 "加载" 模块时预处理文件。

不同的文件类型可能需要匹配不同的loader，做不同的文件转化。而有的模块可能需要处理，有的模块可能需要忽略，这一切相关的配置就在 module 中。

#### rule

module 的主要配置项为 rules 。这是一个数组配置，rules中的每一项rule即配置了如何去处理一个模块。比如我们希望将ES6代码转成ES5代码，这需要引用 babel-loader 。它的rule配置即为：

```javascript
module.exports = {
  //...
  module: {
    rules: [
      {
        test: /\.(js)$/, // 正则匹配已js格式文件
        use: 'babel-loader' // 使用babel-loader来处理这个文件
      }
    ]
  }
};
```

作为 loader 可能会用到其他配置

```javascript
module.exports = {
  //...
  module: {
    rules: [
      {
        test: /\.(js)$/,
        use: {
            loader: 'babel-loader',
            options: {
                presets: ['@babel/preset-env'] // 根据目标浏览器自动转换为相应es5代码
            }
        }
        }
    ]
  }
};
```

如果一个文件需要多次转换，例如我们有个这样的场景需要将less文件转换为css文件，并且less文件的css样式添加前缀等

```javascript
{
  test: /\.(css|less)$/,
  use: [
    'style-loader',
    'css-loader',
    'postcss-loader'
  ]
}
```

##### 参考文章

[《前端九部》Webpack基本配置](https://www.yuque.com/fe9/basic/fnvdeu)