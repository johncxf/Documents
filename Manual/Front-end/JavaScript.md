# JavaScript

参考文档：

- https://www.w3school.com.cn/js/index.asp

## 简介

1995 年，Netscape 公司的 Brenddan Eich 两周内开发设计了 LiveScript 语言，当时为了搭上媒体热炒 Java 的顺风车，临时把 LiveScript 改为为 Javascript。

一个完整的 JavaScript 实现应该由以下三个不同部分组成：

- 核心（ECMAScripts）：JavaScript语言的标准，被称为ECMAScript标准，目前主要有 ES5 和 ES6
- 文档对象模型（DOM）：文档对象模型，是 `HTML` 和`XML` 的应用程序接口（`API`），遵循W3C 的标准，所有浏览器公共遵守的标准
- 浏览器对象模型（BOM）：浏览器对象模型，主要处理浏览器窗口和框架

## DOM

DOM（Document Object Model），文档对象模型

通过 HTML DOM，JavaScript 能够访问和改变 HTML 文档的所有元素。

示例：

```html
<html>
	<body>
        <p id="demo"></p>
        <script>
            document.getElementById("demo").innerHTML = "Hello World!";
        </script>
	</body>
</html>
```

`getElementById` 方法使用 id="demo" 来查找元素，`innerHTML` 属性用于获取或替换 HTML 元素的内容。

### document

#### 查找 HTML 元素

| 方法                                    | 描述                   |
| :-------------------------------------- | :--------------------- |
| document.getElementById(*id*)           | 通过元素 id 来查找元素 |
| document.getElementsByTagName(*name*)   | 通过标签名来查找元素   |
| document.getElementsByClassName(*name*) | 通过类名来查找元素     |

#### 改变 HTML 元素

| 方法                                       | 描述                   |
| :----------------------------------------- | :--------------------- |
| element.innerHTML = *new html content*     | 改变元素的 inner HTML  |
| element.attribute = *new value*            | 改变 HTML 元素的属性值 |
| element.setAttribute(*attribute*, *value*) | 改变 HTML 元素的属性值 |
| element.style.property = *new style*       | 改变 HTML 元素的样式   |

#### 添加和删除元素

| 方法                              | 描述             |
| :-------------------------------- | :--------------- |
| document.createElement(*element*) | 创建 HTML 元素   |
| document.removeChild(*element*)   | 删除 HTML 元素   |
| document.appendChild(*element*)   | 添加 HTML 元素   |
| document.replaceChild(*element*)  | 替换 HTML 元素   |
| document.write(*text*)            | 写入 HTML 输出流 |

#### 添加事件处理程序

| 方法                                                     | 描述                            |
| :------------------------------------------------------- | :------------------------------ |
| document.getElementById(id).onclick = function(){*code*} | 向 onclick 事件添加事件处理程序 |

#### 查找 HTML 对象

| 属性                         | 描述                                        |
| :--------------------------- | :------------------------------------------ |
| document.anchors             | 返回拥有 name 属性的所有 <a> 元素。         |
| document.applets             | 返回所有 <applet> 元素（HTML5 不建议使用）  |
| document.baseURI             | 返回文档的绝对基准 URI                      |
| document.body                | 返回 <body> 元素                            |
| document.cookie              | 返回文档的 cookie                           |
| document.doctype             | 返回文档的 doctype                          |
| document.documentElement     | 返回 <html> 元素                            |
| document.documentMode        | 返回浏览器使用的模式                        |
| document.documentURI         | 返回文档的 URI                              |
| document.domain              | 返回文档服务器的域名                        |
| document.domConfig           | 废弃。返回 DOM 配置                         |
| document.embeds              | 返回所有 <embed> 元素                       |
| document.forms               | 返回所有 <form> 元素                        |
| document.head                | 返回 <head> 元素                            |
| document.images              | 返回所有 <img> 元素                         |
| document.implementation      | 返回 DOM 实现                               |
| document.inputEncoding       | 返回文档的编码（字符集）                    |
| document.lastModified        | 返回文档更新的日期和时间                    |
| document.links               | 返回拥有 href 属性的所有 <area> 和 <a> 元素 |
| document.readyState          | 返回文档的（加载）状态                      |
| document.referrer            | 返回引用的 URI（链接文档）                  |
| document.scripts             | 返回所有 <script> 元素                      |
| document.strictErrorChecking | 返回是否强制执行错误检查                    |
| document.title               | 返回 <title> 元素                           |
| document.URL                 | 返回文档的完整 URL                          |

## BOM

BOM（Browser Object Model），浏览器对象模型，提供与浏览器交互的方法和接口

### window

所有浏览器都支持 *window* 对象

#### 窗口尺寸

- window.innerHeight - 浏览器窗口的内高度（以像素计）
- window.innerWidth - 浏览器窗口的内宽度（以像素计）

浏览器窗口（浏览器视口）不包括工具栏和滚动条。

对于 Internet Explorer 8, 7, 6, 5：

- document.documentElement.clientHeight
- document.documentElement.clientWidth

或

- document.body.clientHeight
- document.body.clientWidth

```javascript
var w = window.innerWidth
|| document.documentElement.clientWidth
|| document.body.clientWidth;

var h = window.innerHeight
|| document.documentElement.clientHeight
|| document.body.clientHeight;
```

#### 其他

- window.open()：打开新窗口
- window.close()：关闭当前窗口
- window.moveTo()：移动当前窗口
- window.resizeTo()：重新调整当前窗口

### screen

window.screen 对象包含用户屏幕的信息

- window.screen.width：返回以像素计的访问者屏幕宽度
- window.screen.height：返回以像素计的访问者屏幕的高度。
- window.screen.availWidth：返回访问者屏幕的宽度，以像素计，减去诸如窗口工具条之类的界面特征
- window.screen.availHeight：返回访问者屏幕的高度，以像素计，减去诸如窗口工具条之类的界面特征
- window.screen.colorDepth：返回用于显示一种颜色的比特数
- window.screen.pixelDepth：返回屏幕的像素深度

前缀可以不带 `window.`

### location

window.location 对象可用于获取当前页面地址（URL）并把浏览器重定向到新页面。

- window.location.href：返回当前页面的 href (URL)
- window.location.hostname：返回 web 主机的域名
- window.location.pathname：返回当前页面的路径或文件名
- window.location.protocol：返回使用的 web 协议（http: 或 https:）
- window.location.assign：加载新文档

### history

window.history 对象包含浏览器历史。

- history.back()：等同于在浏览器点击后退按钮
- history.forward()：等同于在浏览器中点击前进按钮

### navigator

window.navigator 对象包含有关访问者的信息

- window.navigator.appVersion：返回有关浏览器的版本信息
- window.navigator.appName：返回浏览器的应用程序名称
- window.navigator.appCodeName：返回浏览器的应用程序代码名称
- window.navigator.cookieEnabled：返回 true，如果 cookie 已启用，否则返回 false
- window.navigator.product：返回浏览器引擎的产品名称
- window.navigator.userAgent：返回由浏览器发送到服务器的用户代理报头（user-agent header）
- window.navigator.platform

### 弹出框

- window.alert()
- window.confirm()
- window.prompt()

## ES5

ES5，全称 ECMAScript 5，除了 ES6 的新语法，其他 js 语法可以理解为 ES5

## ES6

ES6， 全称 ECMAScript 6.0 ，是 JavaScript 的下一个版本标准，2015.06 发版。

### let和const关键字

ES6)新增加了两个重要的 JavaScript 关键字: **let** 和 **const**。

let 声明的变量只在 let 命令所在的代码块内有效。

const 声明一个只读的常量，一旦声明，常量的值就不能改变。

### 变量解构赋值

针对数组或者对象进行模式匹配，然后对其中的变量进行赋值。

#### 数组（Array）

##### 基本用法

```javascript
let [a, b, c] = [1, 2, 3];
// a = 1
// b = 2
// c = 3
```

##### 其他用法

```javascript
// 嵌套使用
let [a, [[b], c]] = [1, [[2], 3]];
// a = 1
// b = 2
// c = 3

// 忽略变量
let [a, , b] = [1, 2, 3];
// a = 1
// b = 3

// 不完全解构
let [a = 1, b] = [];
// a = 1, b = undefined

let [a, ...b] = [1, 2, 3];
//a = 1
//b = [2, 3]

let [a, b, c, d, e] = 'hello';
// a = 'h'
// b = 'e'
// c = 'l'
// d = 'l'
// e = 'o'

let [a = 2] = [undefined];
// a = 2
```

#### 对象（Object）

##### 基本用法

```javascript
let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
// foo = 'aaa'
// bar = 'bbb'
 
let { baz : foo } = { baz : 'ddd' };
// foo = 'ddd'
```

##### 其他用法

```javascript
// 嵌套变量
let obj = {p: ['hello', {y: 'world'}] };
let {p: [x, { y }] } = obj;
// x = 'hello'
// y = 'world'

// 忽略变量
let obj = {p: ['hello', {y: 'world'}] };
let {p: [x, {  }] } = obj;
// x = 'hello'

// 其他的同数组
```

### Symbol数据类型

ES6 引入了一种新的原始数据类型 Symbol ，表示独一无二的值，最大的用法是用来定义对象的唯一属性名。

ES6 数据类型除了 Number 、 String 、 Boolean 、 Objec t、 null 和 undefined ，还新增了 Symbol 。

#### 基本用法

```javascript
// 定义
let sy = Symbol("KK");
console.log(sy);   // Symbol(KK)
typeof(sy);        // "symbol"
 
// 相同参数 Symbol() 返回的值不相等
let sy1 = Symbol("kk"); 
sy === sy1;       // false
```

#### 其他用法

##### 定义属性名

由于每一个 Symbol 的值都是不相等的，所以 Symbol 作为对象的属性名，可以保证属性不重名。

```javascript
// 定义symbol
let sy = Symbol("key1");
 
// 写法1
let syObject = {};
syObject[sy] = "kk";
console.log(syObject);    // {Symbol(key1): "kk"}
 
// 写法2
let syObject = {
  [sy]: "kk"
};
console.log(syObject);    // {Symbol(key1): "kk"}
 
// 写法3
let syObject = {};
Object.defineProperty(syObject, sy, {value: "kk"});
console.log(syObject);   // {Symbol(key1): "kk"}
```

Symbol 作为对象属性名时不能用.运算符，要用方括号。因为.运算符后面是字符串，所以取到的是字符串 sy 属性，而不是 Symbol 值 sy 属性。

##### 定义常量

用字符串不能保证常量是独特的，这样会引起一些问题，所以可以使用symbol定义常量

##### 内置Symbol值

- Symbol.for()
- Symbol.keyFor()

### 数据结构扩展用法

#### Map和Set数据结构

##### Map

Map 对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。

###### map和object的区别

JavaScript的对象（Object），本质上是键值对的集合（Hash结构），但是传统上只能用字符串当作键。

- 一个 Object 的键只能是字符串或者 Symbols，但一个 Map 的键可以是任意值。
- Map 中的键值是有序的（FIFO 原则），而添加到对象中的键则不是。
- Map 的键值对个数可以从 size 属性获取，而 Object 的键值对个数只能手动计算。
- Object 都有自己的原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。

```javascript
var m = new Map();
var o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
```

##### Set

Set 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。

Set 对象存储的值总是唯一的，所以需要判断两个值是否恒等。有几个特殊值需要特殊对待：

- +0 与 -0 在存储判断唯一性的时候是恒等的，所以不重复；
- undefined 与 undefined 是恒等的，所以不重复；
- NaN 与 NaN 是不恒等的，但是在 Set 中只能存一个，不重复。

```javascript
let mySet = new Set();
 
mySet.add(1); // Set(1) {1}
mySet.add(5); // Set(2) {1, 5}
mySet.add(5); // Set(2) {1, 5} 这里体现了值的唯一性
mySet.add("some text"); 
// Set(3) {1, 5, "some text"} 这里体现了类型的多样性
var o = {a: 1, b: 2}; 
mySet.add(o);
mySet.add({a: 1, b: 2}); 
// Set(5) {1, 5, "some text", {…}, {…}} 
// 这里体现了对象之间引用不同不恒等，即使值相同，Set 也能存储
```

#### Proxy 和 Reflect



#### 字符串



#### 数值



#### 对象



#### 数组



### 函数

#### 默认参数

```javascript
function fn(name,age=17){
 console.log(name+","+age);
}
fn("Amy",18);  // Amy,18
fn("Amy","");  // Amy,
fn("Amy");     // Amy,17
```

### 不定参数

不定参数用来表示不确定参数个数，形如，...变量名，由...加上一个具名参数标识符组成。具名参数只能放在参数组的最后，并且有且只有一个不定参数。

```javascript
function f(...values){
    console.log(values.length);
}
f(1,2);      //2
f(1,2,3,4);  //4
```

#### 箭头函数

箭头函数提供了一种更加简洁的函数书写方式。基本语法是：

```
参数 => 函数体
```

##### 基本用法

```javascript
// 用法1
const f = v => v;

// 用法2
const f = function(a){
 return a;
}

// 用法3：当箭头函数没有参数或者有多个参数，要用 () 括起来
const f = (a,b) => a+b;

// 用法4：当箭头函数函数体有多行语句，用 {} 包裹起来，表示代码块，当只有一行语句，并且需要返回结果时，可以省略 {} , 结果会自动返回。
const f = (a,b) => {
 let result = a+b;
 return result;
}
```

### 迭代器



### class

在ES6中，class (类)作为对象的模板被引入，可以通过 class 关键字定义类。

```javascript
// 匿名类
let Example = class {
    constructor(a) {
        this.a = a;
    }
}
// 命名类
let Example = class Example {
    constructor(a) {
        this.a = a;
    }
}
```

#### 实例化

class 的实例化必须通过 new 关键字。

#### 封装与继承

##### extends

通过 extends 实现类的继承。

```javascript
class Child extends Father { ... }

```

##### super

子类 constructor 方法中必须有 super ，且必须出现在 this 之前。

### 模块



### promise 对象

Promise是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6将其写进了语言标准，统一了用法，原生提供了`Promise`对象。

所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise是一个对象，从它可以获取异步操作的消息。Promise提供统一的API，各种异步操作都可以用同样的方法进行处理。

#### 特点

- 对象的状态不受外界影响。Promise 异步操作有三种状态：pending（进行中）、fulfilled（已成功）和 rejected（已失败）。
- 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise 对象只有：从 pending 变为 fulfilled 和从 pending 变为 rejected 的状态改变。只要处于 fulfilled 和 rejected ，状态就不会再变了即 resolved（已定型）。

#### 基本用法

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由JavaScript引擎提供，不用自己部署。

```javascript
var promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

#### then

then 方法接收两个函数作为参数，第一个参数是 Promise 执行成功时的回调，第二个参数是 Promise 执行失败时的回调，两个函数只会有一个被调用。

### Generator 函数



### async函数

#### 语法

```javascript
async function name([param[, param[, ... param]]]) { statements }
```

- name：函数名称。
- param：要传递给函数的参数的名称。
- statements：函数体语句。

async 函数返回一个 Promise 对象，可以使用 then 方法添加回调函数。

```javascript
async function helloAsync(){
    return "helloAsync";
}
  
console.log(helloAsync())  // Promise {<resolved>: "helloAsync"}
 
helloAsync().then(v=>{
   console.log(v);         // helloAsync
})

```

#### await

await 操作符用于等待一个 Promise 对象, 它只能在异步函数 async function 内部使用。

```javascript
[return_value] = await expression;
```

## 其他

### 事件循环

浏览器是一个多进程、多线程的应用程序。为了避免相互影响，浏览器启动后，会自动创建多个进程。

主要的进程：

1、浏览器进程：主要负责界面交互、用户交互、子进程管理等。

2、网络进程：负责加载网络资源。

3、渲染进程：渲染进程启动后，会开启一个渲染主线程，主线程负责执行HTML、CSS、JS代码。默认情况下，浏览器会为每个标签开启一个新的渲染进程，以保障不同标签页之间不相互影响。

#### 渲染主线程是如何工作的？

渲染主线程是浏览器中最繁忙的线程，需要它处理的任务包括但不限于：

- 解析 HTML
- 解析 CSS
- 计算样式
- 布局
- 处理图层
- 每秒把⻚⾯画 60 次
- 执⾏全局 JS 代码
- 执⾏事件处理函数
- 执⾏计时器的回调函数
- ......

渲染主线程通过排队的方式处理任务

1. 在最开始的时候，渲染主线程会进⼊⼀个⽆限循环
2. 每⼀次循环会检查消息队列中是否有任务存在。如果有，就取出第⼀个任务执⾏，执⾏完⼀个后进⼊下⼀次循环；如果没有，则进⼊休眠状态。
3. 其他所有线程（包括其他进程的线程）可以随时向消息队列添加任务。新任务会加到消息队列的末尾。在添加新任务时，如果主线程是休眠状态，则会将其唤醒以继续循环拿取任务

**整个过程，被称之为事件循环（消息循环）**

#### 如何理解 JS 的异步？

JS是⼀⻔单线程的语⾔，这是因为它运⾏在浏览器的渲染主线程中，⽽渲染主线程只有⼀个。

⽽渲染主线程承担着诸多的⼯作，渲染⻚⾯、执⾏ JS 都在其中运⾏。

如果使⽤同步的⽅式，就极有可能导致主线程产⽣阻塞，从⽽导致消息队列中的很多其他任务⽆法得到执⾏。这样⼀来，⼀⽅⾯会导致繁忙的主线程⽩⽩的消耗时间，另⼀⽅⾯导致⻚⾯⽆法及时更新，给⽤户造成卡死现象。

所以浏览器采⽤异步的⽅式来避免。具体做法是当某些任务发⽣时，⽐如计时器、⽹络、事件监听，主线程将任务交给其他线程去处理，⾃身⽴即结束任务的执⾏，转⽽执⾏后续代码。当其他线程完成时，将事先传递的回调函数包装成任务，加⼊到消息队列的末尾排队，等待主线程调度执⾏。在这种异步模式下，浏览器永不阻塞，从⽽最⼤限度的保证了单线程的流畅运⾏。

#### JS 事件循环

事件循环又叫消息循环，是浏览器主线程的工作方式。

在 chrome 源码钟，它开启一个不会结束的 for 循环，每次循环从消息队列中取出第一个任务执行，而其他线程只需要在合适的时候将任务加入到队列末尾即可。

过去把消息队列简单分为宏队列和微队列，而这种说法目前已无法满足复杂的浏览器环境，取而代之的是一种更加灵活的处理方式。

根据 w3c 官方解释，右浏览器自行决定取哪一个队列的任务。但浏览器必须有一个微队列，微队列的任务一定具有最高优先级，必须优先调度执行。

#### JS 中的计时器能做到精确计时吗？为什么？

不⾏，因为：

1. 计算机硬件没有原⼦钟，⽆法做到精确计时
2. 操作系统的计时函数本身就有少量偏差，由于 JS 的计时器最终调⽤的是操作系统的函数，也就携带了这些偏差
3. 按照 W3C 的标准，浏览器实现计时器时，如果嵌套层级超过 5 层，则会带有 4 毫秒的最少时间，这样在计时时间少于 4 毫秒时⼜带来了偏差
4. 受事件循环的影响，计时器的回调函数只能在主线程空闲时运⾏，因此⼜带来了偏差





