> 第一章~第三章

同一片代码在不同浏览器表现不同，此现象称为"未定义行为"

眼见为实 ： http://demo.cssworld.cn/2/2-1.php

原因 ：FireFox认为`:active`发生在`mousedown`事件之后

<br >

需要注意是，"块级元素"和"display" 为 block 的元素”不是一个概念。

例如，
`<li>`元素默认的 display 值是 list-item，
`<table>`元素默认的 display 值是 table，
但是它们 均是"块级元素"，因为它们都符合块级元素的基本特征，
也就是一个水平流上只能单独显示一个元素，多个块级元素则换行显示。

<br >

伪类选择器: 一般指前面有个英文冒号（:）的选择器，如 `:first-child` 或 `:last-child` 等。

伪元素选择器:就是有连续两个冒号的选择器，如`::first-line`、`::first-letter`、`::before` 和`::after`。

[伪类与伪元素的区别，用法](https://juejin.cn/post/7111867657753722893)

<br >

为什么 `list-item` 元素会出现项目符号？

生成了一个附加的盒子，学名“标记盒子”（marker box），专门用来放圆点、数字这些项目符号。IE
浏览器下伪元素不支持 list-item 或许就是无法创建这个“标记盒子”导致的

<br >

`display: inline-block`

"容器盒子"概念引出。（"外在盒子"与"内在盒子（容器盒子）"）
inline-block : 由外在的"内联盒子"与内在的"块级容器盒子"组成

<br >

width/height作用在哪个盒子上 ？ 

内在盒子

<br >

`width: auto`

(1).充分利用可用空间

(2).收缩与包裹

(3).收缩到最小（表格的一柱擎天效果 http://demo.cssworld.cn/3/2-1.php）

(4).超出容器限制；例如，内容很长的连续的英文和数字，或者内联元素被设置了 white-space:nowrap，则表现为“恰似一江春水向东流，流到断崖也不回头”。

<br >

为何box-sizing不支持margin-box？

`box-sizing`就是改变尺寸作用规则的！

margin 只有在 width 为 auto 的时候可以改变元素的尺寸，但是，此时
元素已经处于流动性状态，根本就不需要 box-sizing。所以，说来说去就是 margin-box
本身就没有价值。

<br >

box-sizing发明的初衷

在 CSS 世界中，唯一离不开 box-sizing:border-box 的就是原生普通文本框`<input>`和文本域`<textarea>`的100%自适应父容器宽度。
`box-sizing` 被发明出来最大的初衷应该是解决`替换元素`（替换元素的特性之一就是尺寸由
内部元素决定，且无论其 `display` 属性值是 `inline` 还是 `block`。这个特性很有意思，对于
非替换元素，如果其 `display` 属性值为 `block`，则会具有流动性，宽度由外部尺寸决定，但是替换元素的宽度却不受 `display` 水平影响，因此，我们通过 CSS 修改`<textarea>`的
`display` 水平是无法让尺寸 100%自适应父容器的）宽度自适应问题。
因此，说来说去，也就 `box-sizing:border-box` 才是根本解决之道！
````css
textarea {
    width: 100%;
    -ms-box-sizing: border-box; /* for IE8 */
    box-sizing: border-box;
}
````

<br >

刚入前端时的写的代码,类似于这样，但是高度为0，页面上啥也不显示
```css
.box {
    width: 100%;
    height: 100%;
    background-color: red;
}
```
对于普通文档流中的元素，百分比高度值要想起作用， 其父级必须有一个可以生效的高度值！

<br >

为何父级没有具体高度值的时候，height:100%会无效？

先看看宽度设置100%的案例 ： https://demo.cssworld.cn/3/2-10.php

因此，当渲染到父元素的时候，子元素的
width:100%并没有渲染，宽度就是图片加文字内容的宽度；等渲染到文字这个子元素的时候，
父元素宽度已经固定，此时的 width:100%就是已经固定好的父元素的宽度。宽度不够怎么
办？溢出就好了，overflow 属性就是为此而生的。

最终的结论只能说是，规定的。。。

<br >

那如何让元素支持height:100%
1.设定显式的高度值
2.使用绝对定位

<br >

任意高度元素的展开收起动画技术

下面代码呈现的效果是生硬地展开和收起：
```css
.element {
    height: 0;
    overflow: hidden;
    transition: height .25s;
}
.element.active {
    height: auto; /* 没有 transition 效果，只是生硬地展开 */
}
```
其中展开后的 `max-height` 值，我们只需要设定为保证比展开内容高度大的值就可以，因为
`max-height` 值比 `height` 计算值大的时候，元素的高度就是 `height` 属性的
计算高度，在本交互中，也就是 `height:auto` 时候的高度值。于是，一个高度
不定的任意元素的展开动画效果就实现了。

眼见为实，http://demo.cssworld.cn/3/3-2.php 

但是，使用此方法也有一点要注意，即虽然说从适用范围讲，`max-height` 值越大使用
场景越多，但是，如果 `max-height` 值太大，在收起的时候可能会有“效果延迟”的问
题，比方说，我们展开的元素高度是 100 像素，而 `max-height` 是 1000 像素，动画时间
是 250 ms，假设我们动画函数是线性的，则前 225 ms 我们是看不到收起效果的，因为
max-height 从 1000 像素到 100 像素变化这段时间，元素不会有区域被隐藏，会给人动
画延迟 225 ms 的感觉，相信这不是你想看到的。
因此，建议 `max-height` 使用足够安全的最小值，这样，收起时即使有延迟，也
会因为时间很短，很难被用户察觉，并不会影响体验。

<br >

`!important` 比直接在元素的 style 属性中设置 CSS 声明还要高

<br >

内联元素
幽灵空白节点
我们可以举一个最简单的例子证明“幽灵空白节点”确实存在， CSS 和 HTML 代码如下：
```css
div {
    background-color: #cd0000;
}
span {
    display: inline-block;
}
```
```html
<div><span></span></div>
```


结果，此`<div>`的高度并不是 0

作祟的就是这里的“幽灵空白节点”，如果我们认为在
`<span>`元素的前面还有一个宽度为 0 的空白字符，是不是一
切就解释得通呢？


规范中实际上对这个“幽灵空白节点”是有提及的，“幽灵空白节点”
实际上也是一个盒子，不过是个假想盒，名叫“strut”，中文直译为“支柱”，是一个存在于每个“行
框盒子”前面，同时具有该元素的字体和行高属性的 0 宽度的内联盒。





