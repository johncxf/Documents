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

### Map和Set数据结构

