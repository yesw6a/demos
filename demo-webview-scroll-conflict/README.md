# 解决使用 webview 时业务内 scroll 与包裹 webview 的外壳 scroll 冲突的问题

> 本 demo 以微信小程序为例

## 演示

![image](https://file.clovemu.com/img/demo-webview-scroll-conflict-01.gif?imgslim)

## 缘起

在小程序内插入 webview 页面，手指滑动页面的时候，没有触发网页内部的滚动，而是出现了整个html页面被下拉，而显示出“网页由xxx.com提供”的字样。

## 分析

webview 页面的 scroll 事件会被包裹在小程序内的 scroll 事件中，导致业务内的 scroll 事件与 webview 的 scroll 事件冲突，导致业务内的 scroll 事件无法触发。

## 解决

1. 禁用页面的 scroll 事件
2. 新增一个 `scroll-view` 组件，包裹在页面的最外层

## 实现

### h5 style

```css
html,
body {
  height: 100%;
  overflow: hidden;
}
```

### scroll-view style

```css
.scroll-view {
  position: fixed;
  position: -ms-device-fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  overflow: auto;
  -webkit-overflow-scrolling: touch; /* 兼容 iOS */
}
```

如果应用了上述样式还不起作用的话，可能是 `header.mate` 需要设置一下。

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

## NOTICE

### 运行 demo

- 使用微信开发者工具运行代码片段
- 使用真机调试，不然无法展示webview
- 使用 node 服务启动 h5
- 修改 `/demo-webview-scroll-conflict/index/index.js` 中 `data.ip` 和 `data.port` 的值为启动 h5 的 `ip` 及端口号