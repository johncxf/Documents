# TypeScript

## 简介

### 概念

> JavaScript的超集。
>
> TypeScript是JavaScript类型的超集，它可以编译成纯JavaScript。

以上是 TypeScript 官网的简介。

**学习资料：**

- https://www.tslang.cn/docs/handbook/basic-types.html
- https://ts.xcatliu.com/introduction/what-is-typescript.html

## 数据类型

### 基础类型

TypeScript支持与JavaScript几乎相同的数据类型，此外还提供枚举、元组、Any、void、never等其他数据类型：

- 布尔值（boolean）
- 数字（number）
- 字符串（string）
- 数组
- 元组
- 枚举（enum）
- Any：声明为 any 的变量可以赋予任意类型的值。
- void：用于标识方法返回值的类型，表示该方法没有返回值。
- null：对象缺失
- undefined：未定义
- never：表示的是那些永不存在的值的类型

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





#### 

