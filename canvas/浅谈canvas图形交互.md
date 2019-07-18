# 浅谈canvas图形交互

总所周知canvas绘制的图形没有交互功能，这篇文章是我这两天基于canvas绘制地铁图总结出来了。

## 圆形交互

> 原理：我们当我们鼠标在canvas滚动的时候，鼠标滚动事件可以获取到`offsetX`和`offsetY`,如果这个坐标点减去圆心坐标点的差大于半径，说明鼠标位于圆形外，反之,鼠标位于圆形内

上代码

首先我们对canvas添加鼠标移动事件，来监听canvas上的鼠标位置，我们把鼠标移动的位置申明在全局作用域下`mouse`对象上

```javascript
const mouse = {
    x: 0,
    y: 0
}

canvas.addEventListener('mousemove', e => {
    const {offsetX, offsetY} = e
    mouse.x = offsetX
    mouse.y = offsetY
})
```

接下来我们申明一个类叫做Point类，包含了两个方法，一个`draw`和`update`

> `update`函数来判断鼠标是否在圆形上面，如果是将该实例的`hover`属性设置为`true`

```javascript
class Circular {
    constructor (x, y, radius) {
        this.x = x
        this.y = y
        this.radius = radius
        this.hover = false // 判断是否移动到上面去
    }
    draw () {
        const {x, y, radius, hover} = this
        ctx.beginPath()
        ctx.arc(x, y, radius, 0, Math.PI * 2, false)
        ctx.strokeStyle = '#222'
        ctx.stroke()
        if (hover) {
            ctx.fillStyle = 'red'
            ctx.fill()
        }
    }
    updata () {
        const {x, y , radius} = this
        const offsetY = mouse.y
        const offsetX = mouse.x
        const isMoveUp = ((x - offsetX)**2 + (y - offsetY)**2) <= (radius ** 2)
        if (isMoveUp) {
            this.hover = true
        } else {
            this.hover = false
        }
        this.draw()
    }
}


const circulares = new Set()
circulares.add(new Point(100, 100, 50))
```

接下来我们顶一个函数`animate`来绘制圆形,并在函数来申明一个`requestAnimationFrame`函数来不停的调用`animate`

```javascript
// 我们将申明的圆形实例放入这个对象中
const circulares = new Set()
circulares.add(new Point(100, 100, 50))
circulares.add(new Point(200, 200, 50))

// 定义animate函数来绘制圆形
function animate () {
    requestAnimationFrame(animate)
    ctx.clearRect(0, 0, width, height)
    for (const circular of circulares) {
        circular.updata()
    }
}
animate()
```

完成代码

```javascript
const mouse = {
    x: 0,
    y: 0
}

canvas.addEventListener('mousemove', e => {
    const {offsetX, offsetY} = e
    mouse.x = offsetX
    mouse.y = offsetY
})

class Point {
    constructor (x, y, radius) {
        this.x = x
        this.y = y
        this.radius = radius
        this.hover = false // 判断是否移动到上面去
    }
    draw () {
        const {x, y, radius, hover} = this
        ctx.beginPath()
        ctx.arc(x, y, radius, 0, Math.PI * 2, false)
        ctx.strokeStyle = '#222'
        ctx.stroke()
        if (hover) {
            ctx.fillStyle = 'red'
            ctx.fill()
        }
    }
    updata () {
        const {x, y , radius} = this
        const offsetY = mouse.y
        const offsetX = mouse.x
        const isMoveUp = ((x - offsetX)**2 + (y - offsetY)**2) <= (radius ** 2)
        if (isMoveUp) {
            this.hover = true
        } else {
            this.hover = false
        }
        this.draw()
    }
}

function animate () {
    requestAnimationFrame(animate)
    ctx.clearRect(0, 0, width, height)
    for (const point of points) {
        point.updata()
    }
}
animate()
```

![demo](https://raw.githubusercontent.com/rwjCxgsy/I-kown/master/canvas/circular.gif)
