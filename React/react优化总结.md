# react 优化总结

react 优化清单：

* 使用事件绑定使用`this.handler`，避免使用`this.handler.bind(this)`和`() => {this.handler}`
* 使用`shouldComponentUpdata`钩子来决定该组件是否重新渲染