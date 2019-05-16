# Rollup 简介

安装

```shell
<!-- 安装 -->
npm install --global rollup

<!-- 新建文件夹 -->
mkdir test-rollup

<!-- 进入文件夹 -->
cd test-rollup

<!-- 初始化项目 -->
npm init
```

我们建立两个文件a.js 和 b.js

a.js

```javascript
export default function () {
    console.log('rollup')
}
```

b.js

```javascript

import hello from 'b.js'

hello()

```

命令行输入

```shell
rollup a.js --format iife --name foo --file bundle.js
```

上面表示

    入口文件a.js
    格式化为 life 模式
    输出包的名字 foo（比如jquery打包后有个jquery对象）
    打包后的文件名叫左bundle.js

我们也可以使用配置文件（rollup.config.js）进行打包

```javascript
export default {
  input: 'a.js', // 入口文件
  output: { // 输出配置
    format: 'iife', // 格式化类型
    name: 'foo', // 输出包名
    file: 'bundle.js' // 输出文件名
  }
}
```

开始打包

```shell
rollup --config rollup.config.js
```

## 使用插件

安装插件

```shell

npm i --save-dev rollup-plugin-uglify

```

rollup.config.js

```javascript

import { uglify } from 'rollup-plugin-uglify'
export default {
  input: 'main.js',
  output: {
    format: 'iife',
    name: 'foo',
    file: 'bundle.js'
  },
  plugins: [ uglify() ]
}
```

### 总结

虽然这篇文章定位为 rollup 简介，但也几乎涵盖了 rollup 的全貌，相较于 webpack 简单得多，我们不需要理解纷繁复杂的概念，配置也简单明了，符合人的直观思维。但 webpack 肯定也有自己的优势，两者定位不同，通常来说，打包 JavaScript 工具、库，推荐使用 rollup，打包 web 应用，推荐使用 webpack。