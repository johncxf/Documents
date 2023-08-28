# TypeScript

## 简介

TypeScript是JavaScript类型的超集，它可以编译成纯JavaScript。

学习文档：

- https://www.tslang.cn/docs/handbook/basic-types.html
- https://ts.xcatliu.com/introduction/what-is-typescript.html

### 安装使用

全局安装：

```shell
$ npm install -g typescript
```

编译 ts 文件：

```shell
$ tsc index.ts
```



## 基础语法

### 基础类型

TypeScript支持与JavaScript几乎相同的数据类型，此外还提供枚举、元组、any、void、never等其他数据类型

#### 布尔值（boolean）

```typescript
let isDone: boolean = false;
```

#### 数字（number）

和JavaScript一样，TypeScript里的所有数字都是浮点数。 这些浮点数的类型是 `number`。 除了支持十进制和十六进制字面量，TypeScript还支持ECMAScript 2015中引入的二进制和八进制字面量。

```typescript
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

#### 字符串（string）

和JavaScript一样，可以使用双引号（ `"`）或单引号（`'`）表示字符串，也可以使用模版字符串，反引号（ `）

```typescript
let name: string = "john";
name = "jonn_chen";

let str = `hello ${name}`;
console.log(str);
```

#### 数组（array）

```typescript
let list: number[] = [1, 2, 3];
// 用数组泛型定义
let list: Array<number> = [1, 2, 3];
```

#### 元组（tuple）

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 `string`和`number`类型的元组。

```typescript
// Declare a tuple type
let x: [string, number];
// OK
x = ['hello', 10];
// Error
x = [10, 'hello'];
```

#### 枚举（enum）

`enum`类型是对JavaScript标准数据类型的一个补充

```typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

#### Any

声明为 any 的变量可以赋予任意类型的值。

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // ok, definitely a boolean
```

#### Void

用于标识方法返回值的类型，表示该方法没有返回值。

```typescript
function warnUser(): void {
    console.log("This is my warning message");
}
```

#### Null

对象缺失

```typescript
let u: undefined = undefined;
```

#### Undefined

未定义

```typescript
let n: null = null;
```

#### Never

表示的是那些永不存在的值的类型

```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

#### Object



### 变量

#### 定义变量

##### 关键字

- let
- const
- var

推荐使用 `let`、`const`

##### 声明变量

```typescript
let [变量名] : [类型] = 值;
```

例：

```typescript
// 声明和初始化
let uname:string = "Runoob";
```

### 接口

#### 定义接口

```typescript
interface interface_name { 
	//
}
```

### 类



### 泛型

泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

```typescript
function reverse<T>(items: T[]): T[] {
  const toreturn = [];
  for (let i = items.length - 1; i >= 0; i--) {
    toreturn.push(items[i]);
  }
  return toreturn;
}

const sample = [1, 2, 3];
let reversed = reverse(sample);

console.log(reversed); // 3, 2, 1

// Safety
reversed[0] = '1'; // Error
reversed = ['1', '2']; // Error

reversed[0] = 1; // ok
reversed = [1, 2]; // ok
```

### 联合类型

用 `|` 作为标记，如 `string | number`）。

一个可以接受字符串数组或单个字符串的函数：

```typescript
function formatCommandline(command: string[] | string) {
  let line = '';
  if (typeof command === 'string') {
    line = command.trim();
  } else {
    line = command.join(' ').trim();
  }
}
```

## 模块

TypeScript1.5中， “内部模块”现在称做“命名空间”，“外部模块”现在则简称为“模块”

#### 导出

导出声明：

```typescript
export const someVar = 123;
export type someType = {
  foo: string;
};

// 或者
const someVar = 123;
type someType = {
  type: string;
};
export { someVar, someType };
```

导出并重命名：

```typescript
const someVar = 123;
export { someVar as aDifferentName };
```

重新导出：

```typescript
export * from "module"
```

#### 导入

```typescript
// 导入一个模块中的某个导出内容
import { something } from "./somefile";

// 对导入内容重命名
import { something as s } from "./somefile";

// 将整个模块导入到一个变量
import * as s from "./somefile";

// 导入模块
import "./somefile";
```

#### 默认导入/导出

每个模块都可以有一个`default`导出。 默认导出使用 `default`关键字标记；并且一个模块只能够有一个`default`导出

默认导出：

```typescript
// some var
export default (someVar = 123);

// some function
export default function someFunction() {}

// some class
export default class someClass {}
```

默认导入：

```typescript
import someName from 'someModule'
```

## 声明文件

### 概念

在 TypeScript 中以 .d.ts  为后缀的文件，我们称之为 TypeScript 声明文件

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。

## 配置文件

`tsconfig.json`文件中指定了用来编译这个项目的根文件和编译选项

使用：

- 不带任何输入文件的情况下调用`tsc`，编译器会从当前目录开始去查找`tsconfig.json`文件，逐级向上搜索父目录
- 不带任何输入文件的情况下调用`tsc`，且使用命令行参数`--project`（或`-p`）指定一个包含`tsconfig.json`文件的目录

当命令行上指定了输入文件时，`tsconfig.json`文件会被忽略。

### 示例

```json
{
    "compilerOptions": {
        "allowJs": true,
        "target": "es5",
        "module": "commonjs",
        "declaration": true,
        "noImplicitReturns": true,
        "lib": ["es6"],
        "outDir": "./dist",
    },
    "include": [
        "src/**/*"
    ],
    "exclude": [
        "node_modules",
        "./test",
        "**/*.spec.ts"
    ]
}
```

可以通过 `compilerOptions` 来定制你的编译选项

`include` 和 `exclude` 选项可以来指定需要包含的文件和排除的文件

### extends

使用`extends`继承配置

`configs/tsconfig.base.json`：

```json
{
  "compilerOptions": {
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}
```

`tsconfig.json`：

```json
{
  "extends": "./configs/tsconfig.base",
  "files": [
    "main.ts",
    "supplemental.ts"
  ]
}
```

### 配置项

#### compilerOptions

```json
{
    "compilerOptions": {
        /* 基本选项 */
        "target": "es5",                       // 指定ECMAScript目标版本: 'ES3' (default), 'ES5', 'ES6'/'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'
        "module": "commonjs",                  // 指定生成哪个模块系统代码: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
        "lib": [],                             // 指定要包含在编译中的库文件
        "allowJs": true,                       // 允许编译 javascript 文件
        "checkJs": true,                       // 报告 javascript 文件中的错误
        "jsx": "preserve",                     // 指定 jsx 代码的生成: 'preserve', 'react-native', or 'react'
        "declaration": true,                   // 生成相应的 '.d.ts' 文件
        "sourceMap": true,                     // 生成相应的 '.map' 文件
        "outFile": "./",                       // 将输出文件合并为一个文件
        "outDir": "./",                        // 指定输出目录
        "rootDir": "./",                       // 用来控制输出目录结构 --outDir.
        "removeComments": true,                // 删除编译后的所有的注释
        "noEmit": true,                        // 不生成输出文件
        "importHelpers": true,                 // 从 tslib 导入辅助工具函数
        "isolatedModules": true,               // 将每个文件作为单独的模块 （与 'ts.transpileModule' 类似）.

        /* 严格的类型检查选项 */
        "strict": true,                        // 启用所有严格类型检查选项
        "noImplicitAny": true,                 // 在表达式和声明上有隐含的 any类型时报错
        "strictNullChecks": true,              // 启用严格的 null 检查
        "noImplicitThis": true,                // 当 this 表达式值为 any 类型的时候，生成一个错误
        "alwaysStrict": true,                  // 以严格模式检查每个模块，并在每个文件里加入 'use strict'

        /* 额外的检查 */
        "noUnusedLocals": true,                // 有未使用的变量时，抛出错误
        "noUnusedParameters": true,            // 有未使用的参数时，抛出错误
        "noImplicitReturns": true,             // 并不是所有函数里的代码都有返回值时，抛出错误
        "noFallthroughCasesInSwitch": true,    // 报告 switch 语句的 fallthrough 错误。（即，不允许 switch 的 case 语句贯穿）

        /* 模块解析选项 */
        "moduleResolution": "node",            // 选择模块解析策略： 'node' (Node.js) or 'classic' (TypeScript pre-1.6)
        "baseUrl": "./",                       // 用于解析非相对模块名称的基目录
        "paths": {},                           // 模块名到基于 baseUrl 的路径映射的列表
        "rootDirs": [],                        // 根文件夹列表，其组合内容表示项目运行时的结构内容
        "typeRoots": [],                       // 包含类型声明的文件列表
        "types": [],                           // 需要包含的类型声明文件名列表
        "allowSyntheticDefaultImports": true,  // 允许从没有设置默认导出的模块中默认导入。

        /* Source Map Options */
        "sourceRoot": "./",                    // 指定调试器应该找到 TypeScript 文件而不是源文件的位置
        "mapRoot": "./",                       // 指定调试器应该找到映射文件而不是生成文件的位置
        "inlineSourceMap": true,               // 生成单个 soucemaps 文件，而不是将 sourcemaps 生成不同的文件
        "inlineSources": true,                 // 将代码与 sourcemaps 生成到一个文件中，要求同时设置了 --inlineSourceMap 或 --sourceMap 属性

        /* 其他选项 */
        "experimentalDecorators": true,        // 启用装饰器
        "emitDecoratorMetadata": true          // 为装饰器提供元数据的支持
    }
}
```

