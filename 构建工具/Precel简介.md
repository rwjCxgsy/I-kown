# Parcel 简易介绍

相对于webpack复杂的配置，Parcel 配置相对于简单的多得多，直接在html中引入一个js入口

Parcel特点

* 极速打包
* 将你所有的资源打包
* 自动转换
* 零配置代码分拆
* 热模块替换
* 友好的错误日志

简单的例子

index.html

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Parcel测试</title>
    </head>
    <body>
        <script src="./index.js"></script>
    </body>
</html>
```

index.js

```javascript
// 引入另一个组件
import main from './main';

main();
```

main.js

```javascript
// 还可以引入css文件
export default function () {
    console.log('main fn')
}
```

安装教程

```shell
# 全局安装
npm i parcel -g
# 创建个文件夹
mkdir demo-parcel
# 进入demo-parcel
cd demo-parcel
# 初始化
npm init -y
# 启动parcel
parcel index.html
# 访问localhost:1234
```

就这么简单，无需要webpack那么复杂的配置

## 资源

```javascript
// 引入 CSS 文件
import './test.css'

// 引入包含 CSS 模块的 CSS 文件
import classNames from './test.css'

// 以 URL 的形式引入图片
import imageURL from './test.png'
```