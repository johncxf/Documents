# HTML

参考文档：

- https://www.w3school.com.cn/h.asp
- https://www.yuque.com/fe9/basic

## 基础

超文本标记语言（Hyper Text Markup Language）也就是HTML

- 标签查询：https://www.w3schools.com/tags/default.asp

### 基本结构

#### 简单示例

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>HTML 示例</title>
        <link rel="stylesheet" type="text/css" href="xxx.css">
        <script src="xxx.js"></script>
    </head>
    <body>
        <h1>我是标题</h1>
        <p>我是段落</p>
    </body>
</html>
```

#### DOCTYPE

`<!DOCTYPE>` 是文档类型声明，它用来告诉浏览器用什么规则解析 HTML 元素

#### html

html 元素在整个 HTML 文档的最外层，它是根元素，包裹着一个完整的页面

#### head

head 元素是头部元素，它包含的内容不会在页面中显示。head 元素可以包含标题信息，元信息，样式表和脚本等。

- `<meta>` 标签提供了页面相关数据信息
- `<title> ` 标签定义了页面的标题
- `<link>` 签通常用来链接一些与页面相关的外部资源，比如 css 文件
- `<style> ` 标签可以直接定义样式信息
- `<script>`标签为页面引入脚本文件，我们可直接使用它的 src 属性，引入脚本文件的地址，也可以直接在页面中插入 JavaScript 代码

#### body

body 元素定义了文档的主体，包含了所有显示在页面上的内容，比如文字，图片，表格，列表，超链接，音频，视频等等。

### HTML 语义化

HTML 语义化是指仅仅从 HTML 元素上就能看出页面的大致结构，比如需要强调的内容可以放在 `<strong> `标签中，而不是通过样式设置 `<span> `标签去做。不同浏览器对 HTML 元素的解析可能有差异，HTML 语义化便是在抛开样式之后，页面能有一个友好的展示效果

## HTML5

### 简介

#### 什么是H5（HTML5）？

- HTML5 是最新的 HTML 标准。
- HTML5 是专门为承载丰富的 web 内容而设计的，并且无需额外插件。
- HTML5 拥有新的语义、图形以及多媒体元素。
- HTML5 提供的新元素和新的 API 简化了 web 应用程序的搭建。
- HTML5 是跨平台的，被设计为在不同类型的硬件（PC、平板、手机、电视机等等）之上运行。

#### H5 新特性

- 新的语义元素，比如 `<header>`、`<footer>`、` <article>`、` <section>`。
- 新的表单控件，比如数字、日期、时间、日历和滑块。
- 强大的图像支持（借由 `<canvas>` 和 `<svg>`）
- 强大的多媒体支持（借由 `<video>` 和 `<audio>`）
- 强大的新 API，比如用本地存储取代 cookie。

#### H5 删除的元素

- `<acronym>`
- `<applet>`
- `<basefont>`
- `<big>`
- `<center>`
- `<dir>`
- `<font>`
- `<frame>`
- `<frameset>`
- `<noframes>`
- `<strike>`
- `<tt>`

### H5 元素

H5 相比于 H4 增加了很多新的元素

参考：https://www.w3school.com.cn/html/html5_new_elements.asp