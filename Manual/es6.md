## ES6

ES6， 全称 ECMAScript 6.0 ，是 JavaScript 的下一个版本标准，2015.06 发版。

### 安转配置

Node.js是JavaScript语言的服务器运行环境，对ES6的支持度比浏览器更高。通过Node，可以体验更多ES6的特性。

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

```
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