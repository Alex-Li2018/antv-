# 渲染机制

![ant/g](./img/antv:g.png)

## 渲染流程

- 选择对应的渲染模式 Renderer, Renderer类里面实现了不同模式下的渲染方法，例如：svg webgl canvas等
- 把渲染模式作为参数传入到Canvas里
```js
new Canvas({
    canvas: $canvas,
    renderer: 渲染的模式
})
```
- 执行构造函数
  - 创建文档this.document = new Document()
  - 合并配置
  - 设置相机位置
  - 设置渲染模式
  - 触发canvas准备好的事件 CanvasEvent.READY

## 创建文档 Documnet

[canvas document](./canvas%20document.md)
