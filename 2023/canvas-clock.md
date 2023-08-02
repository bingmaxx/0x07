# canvas 复刻锤子时钟

> 2023-0801

`#Canvas`

## 介绍
**canvas**：使用脚本 (通常为 JavaScript) 来绘制图形的 HTML 元素。

本人遍历了以下两份文档，学习完就相当于有了笔和纸，至于最后能画出什么，则需要在 canvas 应用方面进一步学习。
- `MDN` 的 **Canvas 教程**[^1]
- 张鑫旭的 **Canvas API 中文文档**[^2]

## Demo 时钟
下面介绍如何使用 canvas 制作一个时钟，首先分析一个简单的时钟包含哪些部分：
- 表盘
- 时针 / 分针 / 秒针
- 按秒走时

**初始化 canvas 画布**
```html
<!-- html -->
<canvas id="clock"></canvas>
```

```css
/* css */
canvas {
  width: 300px;
  height: 300px;
}
```

```js
// js
const radio = window.devicePixelRatio;
const width = 300 * radio;
const height = 300 * radio;
const canvas = document.getElementById('clock');
const ctx = canvas.getContext('2d');
canvas.width = width;
canvas.height = height;
```

`ctx` 对应 “Canvas 2D 渲染上下文”，暴露了大量 API 属性和方法，用于绘制形状，文本，图像和其他对象。

`devicePixelRatio` 是当前显示设备物理像素分辨率与 CSS 像素分辨率之比。假设该值为 **2**，表示浏览器使用 2 个物理像素来绘制 1 个 CSS 像素。在上面代码中，会将 canvas 画布尺寸按照该值放大，使图像更清晰。

**绘制表盘**
```js
/**
 * 绘制表盘
 */
const drawCircle = () => {
  ctx.save();
  ctx.translate(width / 2, height / 2);
  ctx.fillStyle = '#f8f9fa';
  ctx.beginPath();
  ctx.arc(0, 0, 0.4 * width, 0, 2 * Math.PI);
  ctx.fill();
  ctx.restore();
}
```

`save()` 和 `restore()` 可以看作是一对方法，前者保存当前 canvas 画布状态并放在栈最上面，后者将状态依次取出。以 fillStyle 属性为例，代码中 restore 方法执行后，fillStyle 将恢复为之前设置的值。

`translate()` 对 canvas 坐标系进行整体位移，这里用于变换中心点。

`beginPath()` 开始一个新路径，之后由 `arc()` 绘制一个整圆，路径本身没有颜色，可以通过描边或填充为路径着色，代码中使用 `fill()` 方法为整圆填充颜色，作为表盘。

**绘制指针**
```js
/**
 * 绘制单根指针
 * @param {Number} deg 指针沿 12 点钟方向顺时针旋转角度
 * @param {Number} l 指针长度（比例值）
 * @param {String} rgb 指针颜色
 */
const pointer = (deg, l, rgb) => {
  ctx.save();
  ctx.rotate(deg);
  ctx.lineWidth = radio;
  ctx.strokeStyle = rgb;
  ctx.beginPath();
  ctx.moveTo(0, 0);
  ctx.lineTo(0, 0 - l * width);
  ctx.stroke();
  ctx.restore();
}

/**
 * 计算时针、分针、秒针的角度并调用 pointer 函数绘制
 */
const drawPointer = () => {
  const date = new Date();
  const h = date.getHours() % 12;
  const m = date.getMinutes();
  const s = date.getSeconds();
  const h_deg = ((m / 60 + h) / 12) * 2 * Math.PI;
  const m_deg = ((s / 60 + m) / 60) * 2 * Math.PI;
  const s_deg = (s / 60) * 2 * Math.PI;

  ctx.save();
  ctx.translate(width / 2, height / 2);
  pointer(h_deg, 0.2, '#4e4e4e');   // 时针
  pointer(m_deg, 0.25, '#4e4e4e');  // 分针
  pointer(s_deg, 0.35, '#c32927');  // 秒针
  ctx.restore();
}
```

单根指针是由一条指向 12 点钟方向的直线，沿着表盘中心点顺时针旋转指定角度后构成。**drawPointer** 函数计算出三根指针各自与 12 点钟方向的夹角，再调用 **pointer** 函数一一绘制。

**pointer** 函数中，`rotate()` 方法将画布沿着变换后的中心点顺时针旋转指定角度，`moveTo()`、`lineTo()` 方法再绘制一条从中心点指向原 12 点钟方向的路径，使用 `stroke()` 为路径添加描边，如此便完成了一根对应当前时间的指针。

**按秒走时**
```js
setInterval(() => {
  ctx.clearRect(0, 0, width, height);
  drawCircle();
  drawPointer();
}, 1000);
```

每隔 1 秒钟擦除一次画布，然后重新绘制表盘与指针即可。

**效果**

上面代码合起来运行，就能得到一个如下图所示的简单时钟：

![2023_0801_1642_demo_clock](http://md.bingmax.xyz/2023_0801_1642_demo_clock.png)

## Smartisan 时钟
曾经有一部心爱的 **锤子（Smartisan）坚果 R1** 手机，后来……总之手机自带的 **锤子时钟** 我很喜欢，而时间来到 2023 年时，Android 版只剩魅族应用市场在架，版本停留在了 1.4.1，IOS 在 2016-03-25 更新了最后一个版本 1.4.2 后，在 2021-04-14 便一直下架了。

好在我的 iPhone 手机之前下载过，在已购项目中重新下载，打开应用截个图：

![2023_0801_1731_smartisan_clock_ios_0](http://md.bingmax.xyz/2023_0801_1731_smartisan_clock_ios_0.jpg)

在上面代码中，只是实现了一个最简单的 demo 版时钟，现在加一点点细节，复刻一下锤子时钟，效果如下：

![2023_0801_1630_smartisan_clock](http://md.bingmax.xyz/2023_0801_1630_smartisan_clock.png)

代码放在了 [codepen](https://codepen.io/bingmax/full/PoxXqJm)，有兴趣的话请移步。


[^1]: Canvas 教程
https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Basic_usage

[^2]: Canvas API 中文文档
https://www.canvasapi.cn
