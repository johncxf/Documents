# HTML/CSS

参考文档：

- https://www.w3school.com.cn/h.asp

## HTML

超文本标记语言（Hyper Text Markup Language，也就是HTML）

标签查询：https://www.w3schools.com/tags/default.asp

## CSS

层叠样式表（Cascading Style Sheets，也就是CSS）

### 知识点

#### Css 选择器都有什么，权重是怎么计算的

| 选择器       | 用法                  |
| ------------ | --------------------- |
| id选择器     | #myid                 |
| 类选择器     | .myclassname          |
| 标签选择器   | div,h1,p              |
| 相邻选择器   | h1+p                  |
| 子选择器     | ul > li               |
| 后代选择器   | li a                  |
| 通配符选择器 | *                     |
| 属性选择器   | a[rel="external"]     |
| 伪类选择器   | a:hover, li:nth-child |

计算规则：https://blog.csdn.net/qq416761940/article/details/103693379

#### 居中为什么要使用 transform（为什么不使用 marginLeft/marginTop）

transform是独立层，而margin会导致重绘回流

#### 清除浮动的方式

参考：https://juejin.cn/post/6844903504545316877

#### em 和 px 的区别

em：相对长度单位，值不固定，会继承父级元素字体大小，代表倍数

px：绝对长度单位，在缩放页面时无法调整以其作为单位的字体或者按钮

rem：相对长度单位，值不固定，始终基于根元素

#### 行内元素和块级元素有什么区别

- 行内元素水平方向排列，在一条直线上，默认宽度是只与内容有关。
- 块级元素垂直方向排列，各占据一行，默认宽度与父元素一致。
- 块级元素可包含行内元素，行内元素不能包含块级元素，只能包含文本或其它行内元素。
- 行内元素设置 width和height无效，margin上下无效，padding上下无效。line-height有效。