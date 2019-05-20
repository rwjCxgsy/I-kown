# DOM - diff

虚拟DOM这里就不介绍了，这篇主要是记一下dom-diff，比我我们有两个虚拟dom，一个是新的，一个是旧的，如下

```javascript
// var isArray = require("x-is-array")

// 申明一些补丁类型
VPatch.NONE = 0 // 没有发生变化
VPatch.VTEXT = 1 // 文本发生变化
VPatch.VNODE = 2 // 节点发生变化
VPatch.PROPS = 4 // 节点属性发生变化吧
VPatch.ORDER = 5 // 顺序发生变化
VPatch.INSERT = 6 // 节点发生替换
VPatch.REMOVE = 7 // 节点已删除


function VPatch(type, vNode, patch) {
    this.type = Number(type)
    this.vNode = vNode
    this.patch = patch
}

function diff(a, b) {
    var patch = { a: a }
    walk(a, b, patch, 0)
    return patch
}

function walk(a, b, patch, index) {
    if (a === b) {
        return
    }

    var apply = patch[index]
    var applyClear = false
    // 如果新节点不存在
    if (b == null) {
        apply = appendPatch(apply, new VPatch(VPatch.REMOVE, a, b))
    } else if (isVNode(b)) {
        // 判断a是否为vnode
        if (isVNode(a)) {
            // 节点相同
            if (a.tagName === b.tagName &&
                a.key === b.key) {
                // 属性节点补丁判断
                var propsPatch = diffProps(a.properties, b.properties)
                if (propsPatch) {
                    apply = appendPatch(apply,
                        new VPatch(VPatch.PROPS, a, propsPatch))
                }
                // 比较子节点
                apply = diffChildren(a, b, patch, apply, index)
            } else { // 节点不相同
                apply = appendPatch(apply, new VPatch(VPatch.VNODE, a, b))
                applyClear = true
            }
        } else {
            apply = appendPatch(apply, new VPatch(VPatch.VNODE, a, b))
            applyClear = true
        }
    } else if (typeof b === 'string') {
        apply = appendPatch(apply, new VPatch(VPatch.VTEXT, a, b))
    }
    // 补丁存在
    if (apply) {
        patch[index] = apply
    }
}

function diffChildren(a, b, patch, apply, index) {
    var aChildren = a.children
    // 比较节点顺序发生变化没有
    var orderedSet = reorder(aChildren, b.children)
    var bChildren = orderedSet.children

    var aLen = aChildren.length
    var bLen = bChildren.length
    var len = aLen > bLen ? aLen : bLen

    for (var i = 0; i < len; i++) {
        var leftNode = aChildren[i]
        var rightNode = bChildren[i]
        index += 1

        if (!leftNode) {
            if (rightNode) {
                apply = appendPatch(apply,
                    new VPatch(VPatch.INSERT, null, rightNode))
            }
        } else {
            walk(leftNode, rightNode, patch, index)
        }

        if (isVNode(leftNode) && leftNode.count) {
            index += leftNode.count
        }
    }

    if (orderedSet.moves) {
        apply = appendPatch(apply, new VPatch(
            VPatch.ORDER,
            a,
            orderedSet.moves
        ))
    }

    return apply
}

function appendPatch(apply, patch) {
    if (apply) {
        if (isArray(apply)) {
            apply.push(patch)
        } else {
            apply = [apply, patch]
        }

        return apply
    } else {
        return patch
    }
}
```

## 参考

[DOM-Diff](https://github.com/Matt-Esch/virtual-dom/blob/master/vtree/diff.js)