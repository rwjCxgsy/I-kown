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
==publicPath== 表示打包后文件的前缀

### mode （webpack4新增）

模式是webpack@4新增的配置属性，目的是为了简化webpack繁琐的配置。上两节中我们介绍了文件的入口与输出。在一个工程化的项目中：生产环境下，我们希望输出的文件是经过压缩、混淆（丑化）的。而开发环境时由于调试需要，我们希望文件是未压缩与混淆的。

目前已有的模式有：`production` 、`development`(默认) 、`none`

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

### resolve 模块解析

这个配置决定如何去解析模块。（这不是去编译转化文件，不能与上述loader所做的事混淆）如：是否需要缓存此模块；是否自动识别扩展文件名等。不过我们最常用的配置只有一个 alias : 创建 import 或 require 的别名，借此来简写某些模块文件的路径。最常见的如：

```javascript
module.exports = {
  //...
  resolve: {
    alias: {
      '#': path.resolve(process.cwd(), './src');
    }
  }
};
```

这样在某些需要相对路径比较深的模块引入时，可以从这样：

import utils from '../../../utils'

变为这样：

import utils from '#/utils'

### 必备plugin

#### html-webpack-plugin

只要是web应用的工程，必须要有html文件或者其他html模板文件。那么 html-webpack-plugin 就必不可少。这个插件在上述中也有使用过，主要功能就是可以根据项目中的html模板（没有也行），生成想要的html文件，并插入我们构建出来的js与css等资源。

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    pageA: './a.js',
    pageB: './b.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: 'pageA.html',
      chunks: ['pageA'] // 表示pageA.html 引入的是pageA.js这个入口文件
    }),
    new HtmlWebpackPlugin({
      filename: 'pageB.html',
      chunks: ['pageB'] // 表示pageB.html 引入的是pageB.js这个入口文件
    })
  ]
};
```

有一个需要注意的点是，对于构建多页应用来说，每需要一个html文件，就需增加一个 html-webpack-plugin

，并引入相应的chunk

#### mini-css-extract-plugin css文件分离

前我们介绍的配置，一直没有谈及如何抽离css文件。我们的css一直是以 style 标签的形式插入至dom中的。这明显不是我们想要的结果。mini-css-extract-plugin 就是干这样的活的。需要注意的是，如果引入了本插件，那么就需要在css文件的相关 loaders 中需要使用 MiniCssExtractPlugin.loader 替换 style-loader 。
配置举例如下：

```javascript
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
  //...
  plugins: [
    new MiniCssExtractPlugin({
      fileName: '[name].css' // 生产环境需要hash可配置为 [name].[contenthash].css
    })
  ],
  modules: {
    rules: [
      test: /\.css$/,
      use: [
        MiniCssExtractPlugin.loader,
        'css-loader'
      ]
    ]
  }
}
```

#### uglifyjs-webpack-plugin 代码压缩

实现代码压缩、混淆、tree-shaking的。过于常用，故而webpack也已内置。

#### webpack-bundle-analyzer 提供可视化的界面

以用来分析webpack构建后的打包情况。建议工程项目都可以添加，才能清晰掌握自己工程项目具体加载了哪些代码，进而做相应的打包优化。

### 构建配置

#### splitChunks

    该配置项的配置即是 SplitChunkPlugin 的配置，其用处是可以按一定规则将工程中代码提取一部分到一个新的chunk文件。常见的场景有：
    · 把不同页面公共的代码抽离到一个 commonChunk 。
    · 把外部依赖包单独抽离到一个 vendor 。
    · 把React/Vue 等项目必引的库单独抽离成 dll 。

    其基本配置如下：

    name: 分离出的chunk名字，默认为true，chunk name会自动生成。

    maxInitialRequests: 最大可分割出来的chunk数。

    maxAsyncRequests: 最大可分割出来的异步chunk数（按需加载时使用）。

    cacheGroups: 这个配置项是分割代码的核心配置，这是一个对象，key为chunk的唯一识别key，value为chunk的具体分割规则配置，如下：

        name: 分离出的chunk的名字，若未设置且 splitChunks.name 为true时，以chunk的key为name。

        minChunks: 抽离公共代码时，该公共代码最少被几个chunk引用了。

        test: 模块匹配的规则

        priority: 分离规则优先级。有时候一个模块可能被多个规则匹配到，设置优先级可以让某个规则分离的chunk具有更高匹配模块的优先级。

#### 参考文章

[《前端九部》Webpack基本配置](https://www.yuque.com/fe9/basic/fnvdeu)