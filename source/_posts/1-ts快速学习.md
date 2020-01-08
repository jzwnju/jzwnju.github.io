---
title: 1.ts快速学习
tags: TypeScript
categories: 前端
thumbnail: '../assets/img/thumbnails/5.jpg'
date: 2020-01-03 21:06:57
---


## 1.环境搭建
1）安装ts
```
npm install -g typeScript

mkdir ts-study && cd ts-study

mkdir src && touch src/index.ts

npm init

tsc --init
(初始化配置，生成tsconfig.json配置ts文件)
```

<!-- more -->
package.json中加入我们的script命令：
```json
{
  "name": "ts-study",
  "version": "1.0.0",
  "description": "",
  "main": "src/index.ts",
  "scripts": {
    "build": "tsc", // 编译
    "build:w": "tsc -w" // 监听文件，有变动即编译
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "TypeScript ": "^3.6.4"
  }
}
```

tsconfig.json配置文件：
```json
{
  "compilerOptions": {
    "target": "es5",                            // 指定 ECMAScript 目标版本: 'ES5'
    "module": "commonjs",                       // 指定使用模块: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
    "moduleResolution": "node",                 // 选择模块解析策略
    "experimentalDecorators": true,             // 启用实验性的ES装饰器
    "allowSyntheticDefaultImports": true,       // 允许从没有设置默认导出的模块中默认导入。
    "sourceMap": true,                          // 把 ts 文件编译成 js 文件的时候，同时生成对应的 map 文件
    "strict": true,                             // 启用所有严格类型检查选项
    "noImplicitAny": true,                      // 在表达式和声明上有隐含的 any类型时报错
    "alwaysStrict": true,                       // 以严格模式检查模块，并在每个文件里加入 'use strict'
    "declaration": true,                        // 生成相应的.d.ts文件
    "removeComments": true,                     // 删除编译后的所有的注释
    "noImplicitReturns": true,                  // 不是函数的所有返回路径都有返回值时报错
    "importHelpers": true,                      // 从 tslib 导入辅助工具函数
    "lib": ["es6", "dom"],                      // 指定要包含在编译中的库文件
    "typeRoots": ["node_modules/@types"],
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": [                                  // 需要编译的ts文件一个*表示文件匹配**表示忽略文件的深度问题
    "./src/**/*.ts"
  ],
  "exclude": [
    "node_modules",
    "dist",
    "**/*.test.ts",
  ]
}
```

## 2.数据类型
### 1）原始类型
TypeScript的原始类型包括: boolean、number、string、void、undefined、null、symbol、bigint。

注意，开头是小写的，如果你在Typescript文件中写成 Boolean 那代表是 JavaScript 中的布尔对象，这是新手常犯的错误。

#### void:
表示没有任何类型，当一个函数没有返回值时，你通常会见到其返回值类型是 void：
```typescript
function hi(): void {
    console.log("hello world");
}

// 只有null和undefined可以赋给void
const a: void = undefined
```

默认情况下 null 和 undefined 是所有类型的子类型，就是说你可以把 null 和 undefined 赋值给 number 类型的变量。
但是在正式项目中一般都是开启 --strictNullChecks 检测的，即 null 和 undefined 只能赋值给 void 和它们各自。


### 2）顶级类型
#### any:
使用any类型来标记那些在编程阶段还不清楚类型的变量，使他们通过编译阶段的检查,比如来自用户输入或第三方代码库等动态内容。

```typescript
// 慎用any类型，它可能把Typescript变成AnyScript
let b: any = 4;
b = "maybe a string instead";
```

#### unknown
unknown 是 TypeScript 3.0 引入了新类型,是 any 类型对应的安全类型。
unknown 和 any 的主要区别是 unknown 类型会更加严格:在对unknown类型的值执行大多数操作之前,我们必须进行某种形式的检查,而在对 any 类型的值执行操作之前,我们不必进行任何检查。
解释一下，虽然它们都可以是任何类型,但是当 unknown 类型被确定是某个类型之前,它不能被进行任何操作比如实例化、getter、函数执行等等。而 any 是可以的,这也是为什么说 unknown 是更安全的。
如果这么说不够形象，我们看下例子就明白了：

相同点：
```typescript
let value1: any;

value1 = true;             // OK
value1 = 1;                // OK
value1 = "Hello World";    // OK
value1 = Symbol("type");   // OK
value1 = {}                // OK
value1 = [] 

// ***********************************************
let value2: unknown;

value2 = true;             // OK
value2 = 1;                // OK
value2 = "Hello World";    // OK
value2 = Symbol("type");   // OK
value2 = {}                // OK
value2 = []    
```

不同点：
```typescript
let value1: any;

value1.foo.bar;  // OK
value1();        // OK
new value1();    // OK
value1[0][1];    // OK

// *************************************************

let value2: unknown;

value2.foo.bar;  // ERROR
value2();        // ERROR
new value2();    // ERROR
value2[0][1];    // ERROR
```

### 3）底部类型
#### never
never 类型表示的是那些永不存在的值的类型，never 类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是 never 的子类型或可以赋值给 never 类型，即使any也不可以赋值给never。（除了never本身之外）。

example:
```typescript
// 抛出异常的函数永远不会有返回值
function error(message: string): never {
    throw new Error(message);
}

// 空数组，而且永远是空的
const empty: never[] = []
```

### 4)object
```typescript
// 普通对象、枚举、数组、元组通通都是 object 类型
enum Color {
    red = 1
}

let value: object

value = Color
value = [1]
value = [1, 'hello']
value = {}
```


#### 数组
```typescript
// 两种定义方式：
// 1.使用泛型
const arr: Array<number> = [1, 2, 3]

// 2.在元素类型后面接上 []
const arr: number[] = [1, 2, 3]
```

#### 元组（Tuple）
表示已知元素数量和类型，但各元素的类型不必相同的数组。(可看成严格版的数组)
```typescript
let tuple: [string, number];
tuple = ['hello', 10]; // OK

// 元组的元素的数量、类型、顺序必须与定义一致，否则，就会报错。
tuple = ['hello', 10, false] // Error
tuple = ['hello'] // Error
tuple = [10, 'hello']; // Error
```

#### 枚举类型
##### 1）数字枚举（默认）
当我们声明一个枚举类型是,未赋值时默认是数字类型,而且默认从0开始依次累加。
```typescript
enum Color {
    Red,
    Blue,
    Green,
    Yellow
}

console.log(Color.Red === 0); // true
console.log(Color.Blue === 1); // true
console.log(Color.Green === 2); // true
console.log(Color.Yellow === 3); // true
```
给第一个值赋值后，之后的值会根据第一个值进行累加。
```typescript
enum Color {
    Red = 3,
    Blue,
    Green,
    Yellow
}

console.log(Color.Red === 3); // true
console.log(Color.Blue === 4); // true
console.log(Color.Green === 5); // true
console.log(Color.Yellow === 6); // true
```

##### 2）字符串枚举
```typescript
enum Color {
    Red = 'Red',
    Blue = 'Blue',
    Green = 'Green',
    Yellow = 'Yellow'
}

console.log(Color['Red'], Color.Blue); // Red Blue
```

##### 3)异构枚举（数字和字符串混合）
```typescript
enum Hybrid{
    Horse = 0,
    Donkey = "donkey",
}
```

##### 补充：反向映射
JavaScript 对象一般都是正向映射的，即通过key可以取到value，枚举中，不但可以正向映射还可以反向映射。
通过枚举名字获取枚举值，反之，通过枚举值也可以获取到枚举名字
```typescript
enum Color {
    Red = 3,
    Blue,
    Green,
    Yellow
}

console.log(Color.Red); // 3
console.log(Color.[3]); // Red
```

##### 补充：枚举的本质
为什么能够反向映射呢？看完枚举类型被编译成JavaScript之后我们就知道原因了：
```javascript
var Color;
(function (Color) {
    Color[Color["Red"] = 3] = "Red";
    Color[Color["Blue"] = 4] = "Blue";
    Color[Color["Green"] = 5] = "Green";
    Color[Color["Yellow"] = 6] = "Yellow";
})(Color || (Color = {}));
```
原因就是枚举类型编译成特殊结构的对象，（如Color[Color["Red"] = 3] = "Red";）从而同时拥有了正反映射的特性。