# 虚拟dom

> 源代码来之[virtual-dom](https://github.com/Matt-Esch/virtual-dom)，我将一些复杂的代码删了，留下了一些易懂的代码

h函数创建虚拟dom节点函数

```javascript
/*
    tagName: 标签名
    properties: 熟悉集合
    children： 子节点
*/
function h(tagName, properties, children) {
    var childNodes = [];
    var tag = tagName.toUpperCase(), props, key;

    // 如果properties 没有传递，则赋值为空对象
    if (!children && (typeof properties === 'string' || Array.isArray(properties))) {
        children = properties;
        props = {};
    }

    props = props || properties || {};

    // 支持key关键字
    if (props.hasOwnProperty('key')) {
        key = props.key;
        props.key = undefined;
    }

    return new VNode(tag, props, children, key);
}

```

VNode 构造函数实现

```javascript

var noProperties = {}
var noChildren = []

function VNode(tagName, properties, children, key) {
    this.tagName = tagName
    this.properties = properties || noProperties
    this.children = children || noChildren
    this.key = key != null ? String(key) : undefined
}

```

我们来构造一个虚拟dom

```javascript

var tree = h('div', [
    h('span', 'some text'),
    h('input', { type: 'text', value: 'foo' })
])

```