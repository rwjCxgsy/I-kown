# DOM - diff

虚拟DOM这里就不介绍了，这篇主要是记一下dom-diff，比我我们有两个虚拟dom，一个是新的，一个是旧的，如下

```javascript
const old = CreateElement('div', {
    id: 'app',
}, children: [
    CreateELement('p' null, '列表')
    CreateELement('ul', null, children: [
        CreateELement('li', null, 'java')
        CreateELement('li', null, 'node')
        CreateELement('li', null, 'python')
    ])
])

const old = CreateElement('div', {
    id: 'app',
}, children: [
    '1',
    CreateELement('p' {
        className: 'list'
    }, '语言列表')
    CreateELement('ul', null, children: [
        CreateELement('li', null, 'javascript')
        CreateELement('li', null, 'node')
        CreateELement('li', null, 'python')
    ])
])
```

比较两个dom树，我们采用深度优先遍历来实现

比较节点存在以下几种情况

* 文本节点发生变化`{type: "TEXT", text: newTree}`
* 属性发生变化`{type: "ATTR", attr: attr}`
* 不存在新节点`{type: "REMOVE", index}`
* 节点不一样`{type: "REPLACE", newNode}`

diff.js

```javascript
export function diff (oldTree, newTree) {
    // 声明变量patches用来存放补丁的对象
    const patches = {};
    // 第一次比较应该是树的第0个索引,也就是根节点
    let index = 0;
    // 递归树 比较后的结果放到补丁里
    walk(oldTree, newTree, index, patches);
    return patches;
}

function walk (oldTree, newTree, index, patches) {
    // 每个元素都有一个补丁
    const current = []
    // 表示该节点已经不存在，移除了
    if (!newTree) {
        current.push({
            type: "REMOVE",
            index
        })
    } else if (typeof oldTree === 'string' && typeof newTree === 'string') {
        // 判断两个节点值是否一致
        if (oldTree !== newTree) {
            current.push({
                type: 'TEXT',
                text: newTree
            })
        }
    } else if (oldTree.type === newTree.type) {
        // 比较属性是否有更改
        let attr = diffAttr(oldNode.props, newNode.props);
        if (attr) {
            current.push({ type: 'ATTR', attr });
        }
        // 如果有子节点，遍历子节点
        diffChildren(oldNode.children, newNode.children, patches);
    } else {
        // 表示该节点已经被替换
        current.push({ type: 'REPLACE', newNode});
    }
    // 当前元素确实有补丁存在
    if (current.length) {
        // 将元素和补丁对应起来，放到大补丁包中
        patches[index] = current;
    }
}

// 所有都基于一个序号来实现
let num = 0;
// 比较子节点
function diffChildren(oldChildren, newChildren, patches) {
    // 比较老的第一个和新的第一个
    oldChildren.forEach((child, index) => {
        walk(child, newChildren[index], ++num, patches);
    });
}


// 比较属性
function diffAttr(oldAttrs, newAttrs) {
    let patch = {};
    // 判断老的属性中和新的属性的关系
    for (let key in oldAttrs) {
        if (oldAttrs[key] !== newAttrs[key]) {
            patch[key] = newAttrs[key]; // 有可能还是undefined
        }
    }

    for (let key in newAttrs) {
        // 老节点没有新节点的属性
        if (!oldAttrs.hasOwnProperty(key)) {
            patch[key] = newAttrs[key];
        }
    }
    return Object.keys(patch).length ? patch : null;
}
```

## 参考

[珠峰 DOM-Diff算法学习（提取码rtcu）](https://pan.baidu.com/share/init?surl=I0glERv96CxaBmCvDSRo5g)