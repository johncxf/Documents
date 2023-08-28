# CSS

层叠样式表（Cascading Style Sheets，也就是CSS）

## 属性

参考：https://www.w3school.com.cn/cssref/index.asp

## 盒模型

在网页布局中，我们可以将 HTML 标签看成一个个矩形盒子，盒模型就是用来描述这些矩形盒子所占的空间大小。

#### 相关属性

##### border

设置元素边框

border 是 `border-width`、`border-style`、`border-color` 的简写，分别用来设置边框的宽度，样式（实线、虚线、双线等），颜色。

示例：

```css
.box {
    border: 5px solid #94E8FF;
}
```

##### padding

设置所有内边距属性

padding 是 `padding-top`、`padding-right`、`padding-bottom`、`padding-left` 的简写，分别指上内边距、右内边距、下内边距和左内边距。

示例：

```css
padding: 10px;    /*上、下、左、右内边距均为10px*/
padding: 10px 20px;  /*上、下内边距为10px，左右内边距为20px*/
padding: 10px 20px 30px;  /*上内边距为10px，左、右内边距均为20px，下内边距为30px*/
padding: 10px 20px 30px 40px;  /*上内边距为10px，右内边距为20px，下内边距为30px，左内边距为40px*/
```

##### margin

设置元素外边距属性

margin 是 `margin-top`、`margin-right`、`margin-bottom`、`margin-left` 的简写，同 padding 一样，margin 值也有多种设置方法，不同写法对应的值参照 padding。

#### 盒模型类型

在标准盒模型下通过设置 `box-sizing: border-box;`  可转换为 IE 盒模型。

示例：

```css
/* 标准盒模型 */
.box_1 {
    width: 100px;
    height: 100px;  
    background-color: #FFB5BF;
    border: 5px solid #94E8FF;
    padding: 10px 20px 30px;
    margin: 10px;
 }

/* IE盒模型 */
.box_2 {
    width: 100px;
    height: 100px;  
    background-color: #FFB5BF;
    border: 5px solid #94E8FF;
    padding: 10px 20px 30px;
    margin: 10px;
    box-sizing: border-box;   /* 较 box_1 新增加的属性 */
}
```

##### 标准盒模型

- 元素的 width、height 只包含内容 content，不包含 border 和 padding 值；
- 盒子的大小由元素的宽高、边框和内边距决定。

盒子的框高计算：

- 盒子的宽 = `width` + `border-width` * 2 + `padding-left` + `padding-right`
- 盒子的高 = `height` + `border-width` * 2 + `padding-top` + `padding-bottom`

##### IE盒模型

- 元素的 width、height 不仅包括 content，还包括 border 和 padding；
- 盒子的大小取决于 width、height，修改 border 和 padding 值不能改变盒子的大小。

盒子的框高计算：

- 盒子的宽 = `width`
- 盒子的高 = `height`

## 布局

#### 相关属性

##### display

display 属性规定元素应该生成的框的类型。

属性值有：`block`、`inline`、`inline-block`、`inherit`、`none`、`flex`。inherit 表示这个元素从父元素继承 display 属性值；none 表示这个元素不显示，也不占用空间位置；flex 是flex 布局重要的属性设置

每个元素都有默认的 display 属性，比如 div 标签的默认 display 属性是 block，我们通常称这类元素为**块级元素**；span 标签的默认 display 属性是 inline，我们通常称这类元素为**行内元素**

块级元素：

- 没有设置宽度时，它的宽度是其容器的 100%；
- 可以给块级元素设置宽高、内边距、外边距等盒模型属性；
- 块级元素可以包含块级元素和行内元素；
- 常见的块级元素有：`<div>`、`<h1>` ~ `<h6>`、`<p>`、`<ul>`、`<ol>`、`<dl>`、`<table>`、`<address>`、`<form>` 等。

行内元素：

- 行内元素不会独占一行，只会占领自身宽高所需要的空间；
- 给行内元素设置宽高不会起作用，margin 值只对左右起作用，padding 值也只对左右起作用；
- 行内元素一般不可以包含块级元素，只能包含行内元素和文本；
- 常见的行内元素有 `<a>`、`<b>`、`<label>`、`<span>`、`<img>`、`<em>`、`<strong>`、`<i>`、`<input>` 等。

img 标签设置宽高是可以影响图片大小的，这是因为 img 是可替代元素，可替代元素具有内在的尺寸，所以宽高可以设定。HTML 中的 input、button、textarea、select 都是可替代元素，这些元素即使是空的，浏览器也会根据其标签和属性来决定显示的内容。

inline-block 属性既具有块级元素可以设置宽高的特性，又具有行内元素不换行的特性

##### position

规定元素的定位类型

- relative：相对定位，相对于元素的正常位置进行定位；
- absolute：绝对定位，相对于除 static 定位以外的元素进行定位；
- fixed：固定定位，相对于浏览器窗口进行定位，网站中的固定 header 和 footer 就是用固定定位来实现的；
- static：默认值，没有定位属性，元素正常出现在文档流中；
- inherit：继承父元素的 position 属性值。

##### float

float 属性定义元素在哪个方向浮动，常用属性值有 `left`、`right`，即向左浮动和向右浮动。设置了 float 的元素，会脱离文档流，然后向左或向右移动，直到碰到父容器的边界或者碰到另一个浮动元素。块级元素会忽略 float 元素，文本和行内元素却会环绕它，所以 float 最开始是用来实现文字环绕效果的。

#### 经典布局

##### 两栏布局



##### 三栏布局

圣杯布局：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>圣杯布局</title>
    <style>
        body {
            min-width: 630px;
        }
        .container {
            overflow: hidden;
            padding-left: 100px;
            padding-right: 200px;
        }
        .center {
            width: 100%;
            height: 150px;
            background-color: #94E8FF;
            float: left;
        }
        .left {
            width: 100px;
            height: 150px;
            background-color: #FFB5BF;
            float: left;
            margin-left: -100%;
            position: relative;
            left: -100px;
        }
        .right {
            width: 200px;
            height: 150px;
            background-color: #8990D5;
            float: left;
            margin-left: -200px;
            position: relative;
            right: -200px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="center">center</div>
        <div class="left">left</div>
        <div class="right">right</div>
    </div>
</body>
</html>
```

双飞翼布局：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>双飞翼布局</title>
    <style>
        body {
            min-width: 630px;
        }
        .container {
            overflow: hidden;
        }
        .center-container {
            width: 100%;
            float: left;
        }
        .center-container .center {
            height: 150px;
            background-color: #94E8FF;
            margin-left: 100px;
            margin-right: 200px;
        }
        .left {
            width: 100px;
            height: 150px;
            background-color: #FFB5BF;
            float: left;
            margin-left: -100%;
        }
        .right {
            width: 200px;
            height: 150px;
            background-color: #8990D5;
            float: left;
            margin-left: -200px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="center-container">
            <div class="center">center</div>
        </div>
        <div class="left">left</div>
        <div class="right">left</div>
    <div>
</body>
</html>
```

### Flex 布局

lexbox（全称 Flexible Box）布局，也叫 Flex 布局，意为“弹性布局”，顾名思义，Flex 布局中的元素具有可伸缩性，是的，通过设置父元素的 display 属性为 `display: flex | inline-flex;` 其子元素便有了伸缩性，即使在子元素的宽高不确定的情况下，也能通过设置相关 css 属性来决定子元素的对齐方式、所占比例和空间分布

#### 容器属性

display 属性用来将父元素定义为 Flex 布局的容器，设置 display 值为 `display: flex;` 容器对外表现为块级元素；`display: inline-flex;` 容器对外表现为行内元素，对内两者表现是一样的。

容器包含六个属性：

- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

##### flex-direction

flex-direction 定义了主轴的方向，即项目的排列方向。

```html
<div class="container">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
</div>
```

```css
.container {
    flex-direction: row | row-reverse | column | column-reverse;
}
```

- row（默认值）：主轴在水平方向，起点在左侧，也就是我们常见的从左到右；
- row-reverse：主轴在水平方向，起点在右侧；
- column：主轴在垂直方向，起点在上沿；
- column-reverse: 主轴在垂直方向，起点在下沿。

##### flex-wrap

默认情况下，项目是排成一行显示的，flex-wrap 用来定义当一行放不下时，项目如何换行。

```css
.container {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```

假设此时主轴是从左到右的水平方向：

- nowrap（默认）：不换行
- wrap：换行，第一行在上面
- wrap-reverse：换行，第一行在下面

##### flex-flow

flex-flow 是 flex-direction 和 flex-wrap 的简写，默认值是 row no-wrap。

```css
.container {
    flex-flow: <flex-direction> || <flex-wrap>;
}
```

##### justify-content

justify-content 定义了项目在主轴上的对齐方式。

```css
.container {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

- flex-start（默认）：与主轴的起点对齐；
- flex-end：与主轴的终点对齐；
- center：项目居中；
- space-between：两端对齐，项目之间的距离都相等；
- space-around：每个项目的两侧间隔相等，所以项目与项目之间的间隔是项目与边框之间间隔的两倍。

##### align-items

align-items 定义了项目在交叉轴上如何对齐。

```css
.container {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```

- flex-start：与交叉轴的起点对齐；
- flex-end：与交叉轴的终点对齐；
- center：居中对齐；
- baseline：项目第一行文字的基线对齐；
- stretch（默认值）：如果项目未设置高度或者为 auto，项目将占满整个容器的高度。

##### align-content

align-content 定义了多根轴线的对齐方式，若此时主轴在水平方向，交叉轴在垂直方向，align-content 就可以理解为多行在垂直方向的对齐方式。项目排列只有一行时，该属性不起作用。

```css
.container {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

- flex-start：与交叉轴的起点对齐；
- flex-end： 与交叉轴的终点对齐；
- center：居中对齐；
- space-between：与交叉轴两端对齐，轴线之间的距离相等；
- space-around：每根轴线两侧的间隔都相等，所以轴线与轴线之间的间隔是轴线与边框之间间隔的两倍；
- stretch（默认值）：如果项目未设置高度或者为 auto，项目将占满整个容器的高度

### 项目属性

项目包含六个属性：

- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self

#### order

order 定义了项目的排列顺序，默认值为 0，数值越小，排列越靠前。

```css
.item {
    order: <integer>;
}
```

#### flex-grow

flex-grow 定义了项目的放大比例，默认为 0，也就是即使存在剩余空间，也不会放大。

如果所有项目的 flex-grow 都为 1，则所有项目平分剩余空间；如果其中某个项目的 flex-grow 为 2，其余项目的 flex-grow 为 1，则前者占据的剩余空间比其他项目多一倍。

```css
.item {
    flex-grow: <number>;  
}
```

#### flex-shrink

flex-shrink 定义了项目的缩小比例，默认为 1，即当空间不足时，项目会自动缩小。

如果所有项目的 flex-shrink 都为 1，当空间不足时，所有项目都将等比缩小；如果其中一个项目的 flex-shrink 为 0，其余都为 1，当空间不足时，flex-shrink 为 0 的不缩小。

负值对该属性无效。

```css
.item {
    flex-shrink: <number>；
}
```

#### flex-basis

flex-basis 定义了在分配多余的空间之前，项目占据的主轴空间，默认值为 auto，即项目原来的大小。浏览器会根据这个属性来计算主轴是否有多余的空间。

flex-basis 的设置跟 width 或 height 一样，可以是像素，也可以是百分比。设置了 flex-basis 之后，它的优先级比 width 或 height 高。

```css
.item {
    flex-basis: <length> | auto;
}
```

#### flex

flex 属性是 flex-grow、flex-shrink、flex-basis 的缩写，默认值是 0 1 auto，后两个属性可选。

该属性有两个快捷值：auto（1 1 auto）和 none（0 0 auto）。auto 代表在需要的时候可以拉伸也可以收缩，none 表示既不能拉伸也不能收缩。

```css
.item {
    flex: auto | none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

#### align-self

align-self 用来定义单个项目与其他项目不一样的对齐方式，可以覆盖 align-items 属性。默认属性值是 auto，即继承父元素的 align-items 属性值。当没有父元素时，它的表现等同于 stretch。

align-self 的六个可能属性值，除了 auto 之外，其他的表现和 align-items 一样。

```css
.item {
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

### 示例

#### 三栏布局

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>三栏布局</title>
    <style>
        .container {
            display: flex;
        }
        .center {
            height: 150px;
            background-color: #94E8FF;
            flex-grow: 1;
        }
        .left {
            width: 100px;
            height: 150px;
            background-color: #FFB5BF;
            order: -1;
        }
        .right {
            width: 200px;
            height: 150px;
            background-color: #8990D5;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="center">center</div>
        <div class="left">left</div>
        <div class="right">right</div>
    </div>
</body>
</html>
```



## 知识点

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