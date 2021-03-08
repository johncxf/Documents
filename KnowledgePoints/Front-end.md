# 前端

前端知识点：css、JavaScript、node、ES6...

## CSS

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

## JavaScript

#### 怎么判断js的数据类型？

-  typeof

  可以判断出'string','number','boolean','undefined','symbol'
  但判断 typeof(null) 时值为 'object'; 判断数组和对象时值均为 'object'

- instanceof

  原理是 构造函数的 prototype 属性是否出现在对象的原型链中的任何位置

  ```
  function A() {}
  let a = new A();
  a instanceof A     //true,因为 Object.getPrototypeOf(a) === A.prototype;
  ```

- Object.prototype.toString.call()

  常用于判断浏览器内置对象，对于所有基本的数据类型都能进行判断，即使是 null 和 undefined

- Array.isArray()

  用于判断是否为数组

#### ES5 和 ES6 分别几种方式声明变量？

ES5 有俩种：`var` 和 `function`
ES6 有六种：增加四种，`let`、`const`、`class` 和 `import`

注意：`let`、`const`、`class`声明的全局变量再也不会和全局对象的属性挂钩

#### var、let、const 的区别



#### 闭包的概念以及优缺点？

> 闭包就是能读取其他函数内部变量的函数。

优点：

1. 避免全局变量的污染
2. 希望一个变量长期存储在内存中（缓存变量）

缺点：

1. 内存泄露（消耗）
2. 常驻内存，增加内存使用量

#### async 和 await



#### javascript 的垃圾回收机制



#### 对前端性能优化有什么了解？一般都通过那几个方面去优化的？

- 减少请求数量
- 减小资源大小
- 优化网络连接
- 优化资源加载
- 减少重绘回流
- 性能更好的API
- webpack优化

## Node

#### koa2 和 express 区别



#### node 的适用场景以及优缺点是什么？



#### node 如何和 MySQL 进行通信

npm install mysql

## webpack

#### 介绍下 webpack，并说下 Webpack 的构建流程

#### 请说明 JavaScript 进行压缩、合并、打包实现的原理是什么？为什么需要压缩、合并、打包？

#### 请说出前端框架设计模式MVVM的含义以及原理

## VUE

#### Vue 组件间通信有哪些方式?

1. props/$emit
2. $emit/$on
3. vuex
4. $attrs/$listeners
5. provide/inject
6. $parent/$children 与 ref