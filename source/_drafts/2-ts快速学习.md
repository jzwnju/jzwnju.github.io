---
title: 2-ts快速学习
tags: TypeScript
categories: 前端
thumbnail: '../assets/img/thumbnails/6.jpg'
date: 2020年01月06日21:42:32
---

**接口、类、函数、泛型**

## 1.接口 “：”
接口是一系列抽象方法的声明，是一些方法特征的集合，是为这些类型命名和为你的代码或第三方代码定义契约。

通俗的说定义类型。


### 1）基本用法
例如，一个函数接收某个对象User作为参数，我们需要描述其类型，但它不是基本类型，这时候就需要用接口interface来描述这个类型。

```typescript
interface User {
    name: string
    age: number
    isMale: boolean
}

const getUserName = (user: User) => user.name
```

上面这种写法只检查user的结构及属性的类型（不检查顺序）。

<!-- more -->

### 2）属性可选
如果某个属性是可选的，例如age，用如下写法：
```typescript
interface User {
    name: string
    age?: number
    isMale: boolean
}
```

### 3）属性只读
该属性前加readonly
```typescript
interface User {
    name: string
    age?: number
    readonly isMale: boolean
}
```

### 4）属性是函数类型
两种方式描述：
##### （1）内部描述
```typescript
interface User {
    name: string
    age?: number
    readonly isMale: boolean
    say: (words: string) => string
}
```

##### (2)先用接口描述函数类型，再在对象内部使用
```typescript
interface Comment {
    (words: string) : string
}

interface User {
    name: string
    age?: number
    readonly isMale: boolean
    comment: Comment
}
```

### 5）可索引类型（属性对象成员的个数不定）
例如contact的个数不定
```typescript
// 李雷的信息
{
    name: 'Lilei',
    age: 18,
    isMale: true,
    say: Function,
    contact: {
        NetEase: 'Lilei@163.com',
        qq: 'Lilei@qq.com',
        sina: 'Lilei@sina.com',
    }
}

// 韩梅梅的信息
{
    name: 'Hanmeimei',
    age: 18,
    isMale: true,
    say: Function,
    contact: {
        qq: 'Hanmeimei@qq.com',
        sina: 'Hanmeimei@sina.com',
    }
}
```
contact的key和value都是string类型的，但数量不等。
我们用可索引类型可以这样描述：
```typescript
interface Contact {
    [name:string]: string
}

interface User {
    name: string
    age?: number
    readonly isMale: boolean
    say: () => string
    contact: Contact
}
```

### 6) 接口的继承
如果要继承新接口，例如，VIPUser继承普通User，额外多了一些属性，我们可以使用继承：
```typescript
interface VIPUser extends User {
    premission: () => void
}
```

继承多个接口时：
```typescript
interface VIPUser extends User, SupperUser {
    premission: () => void
}
```

### 7) 接口属性的检查
对象字面量当被赋值给变量或作为参数传递的时候，如果一个对象字面量存在任何“目标类型”不包含的属性时，你会得到一个错误：
'xxx（属性）' not expected in type 'xxx（接口）'
举个🌰：
```typescript
interface Config {
    userid?: string
}

function beVIPUser(config: Config): { user: VIPUser} {
    // ......
    return { user: VIPUser}
}

// 假设属性名称写错
let user = beVIPUser({ userID: 12345678 }) // error: 'userID' not expected in type 'Config'
```

解决由传对象字面量而额外检查属性的方法的办法有以下3种：
（1）使用类型断言
```typescript
let user = beVIPUser({ userID: 12345678 } as Config)
```

（2）添加字符串索引签名
```typescript
interface Config {
    userid?: string;
    [propName: string]: any;
}
```

（3）将字面量赋值给另外一个变量（不建议使用）
```typescript
let conf = { userID: 12345678 }
let user = beVIPUser(conf)
```


## 2.类（class）
es6已经有了的class特性如静态属性和方法、继承等这里不过多讨论，仅介绍typescript所特有的一些特性：
### 1）抽象类
抽象类做为其它派生类的基类使用,它们一般不会直接被实例化,不同于接口,抽象类可以包含成员的实现细节。
实例化抽象类会报错，通常某个子类继承某个抽象类，然后再实例化子类。
abstract 关键字是用于定义抽象类和在抽象类内部定义抽象方法。

使用举例：
```typescript
abstract class Animal {
    abstract eat(): void;
    move(): void {
        console.log('一二一')
    }
}

class Cat extends Animal {
    eat() {
        console.log('🐭真香！')
    }
}

const cat = new Cat()

cat.eat() // 🐭真香！
cat.move() // 一二一
```

### 2）访问限定符
TypeScript 中有三类访问限定符，分别是: public、private、protected。
#### (1)public
成员默认都是public，此限定符修饰的成员可被外部访问。

#### (2)private
当成员被设置为private之后，该限定符成员只能被类内部访问。（实例不能访问）

#### (3)protected
当成员被设置为 protected 之后, 被此限定符修饰的成员是只可以被类的内部以及类的子类访问。（实例不能访问）

### 3）class可以作为interface
react工程常见这种用法。


## 3.函数
### 1）函数的定义
在 JavaScript 中，有两种常见的定义函数的方式——函数声明（Function Declaration）和函数表达式（Function Expression）：
```javascript
// 函数声明（Function Declaration）
function sum(x, y) {
    return x + y;
}

// 函数表达式（Function Expression）
let mySum = function (x, y) {
    return x + y;
};
```

下面分别介绍两种类型的ts函数声明：
#### （1）函数声明
约束，需要把输入和输出都考虑到：
```typescript
function sum(x: number, y: number): number {
    return x + y;
}
```
需要注意的是，输入多余的（或者少于要求的）参数，是不被允许的，会报错。

#### （2）函数表达式
```typescript
let mySum = function (x: number, y: number): number {
    return x + y;
};
```
上面这种写法是可以通过编译的，不过事实上，上面的代码只对等号右侧的匿名函数进行了类型定义，而等号左边的 mySum，是通过赋值操作进行**类型推断**而推断出来的。如果需要我们手动给 mySum 添加类型，则应该是这样：
```typescript
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```
这里需要解释的是：注意不要混淆了 TypeScript 中的 => 和 ES6 中的 =>。
在 ES6 中，=> 叫做箭头函数。
在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。

### 2）函数参数
#### （1）可选参数
在参数后面加上 ? 即代表参数可能不存在。
```typescript
const add = (a: number, b?: number) => a + (b ? b : 0)
```

#### （2）默认参数
参数后使用=赋值即可。
```typescript
const add = (a: number, b = 10) => a + b
```

#### （3）剩余参数
ES6 中，可以使用 ...item 的方式获取函数中的剩余参数。
```javascript
function push(array, ...items) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

事实上，items 是一个数组。所以typescript可以用数组的类型来定义它。
```typescript
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

### 3）重载
重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。
比如，我们需要实现一个函数 reverse，输入数字 123 的时候，输出反转的数字 321，输入字符串 'hello' 的时候，输出反转的字符串 'olleh'。
利用联合类型，我们可以这么实现：
```typescript
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```
然而这样写有一个缺点，就是不能够精确的表达，输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串。

这时，我们可以使用重载定义多个 reverse 的函数类型：
```typescript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```
上例中，我们重复定义了多次函数 reverse，前几次都是函数定义，最后一次是函数实现。
注意，TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。


## 4.泛型
### 1）含义和基本使用
类型变量在 TypeScript 中就叫做「泛型」。
举个🌰：
假设我们用一个函数，它可接受一个 number 参数并返回一个 number 参数。
```typescript
function returnItem (para: number): number {
    return para
}
```
如果我们要接受一个 string 并返回同样一个 string 呢？逻辑是一样的，但是仅仅是类型发生了变化，难道要再写一遍？
```typescript
function returnItem (para: string): string {
    return para
}
```
为了避免这种重复性代码，而不想用any：
```typescript
function returnItem (para: any): any {
    return para
}
```

实际上，我们真正的需求是在静态编写的时候并不确定传入的参数到底是什么类型，只有当在运行时传入参数后我们便能确定参数是什么类型。

**那么我们需要变量，这个变量代表了传入的类型，然后再返回这个变量，它是一种特殊的变量，只用于表示类型而不是值，这就是泛型。**
```typescript
我们在函数名称后面声明泛型变量 <T>，它用于捕获开发者传入的参数类型（比如说string），然后我们就可以使用T(也就是string)做参数类型和返回值类型了。
```

#### 2）多个类型参数
定义泛型的时候，可以一次定义多个类型参数，比如我们可以同时定义泛型 T 和 泛型 U。
```typescript
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]
```

#### 3）泛型变量
泛型除了作为整个类型来使用，还可以作为一部分类型来使用。
怎么理解呢？
假设有这样的需求，我们的函数接受一个数组，如何把数组的长度打印出来，最后返回这个数组，我们应该如何定义？
```typescript
function getArrayLength<T>(arg: T): T {
  console.log(arg.length) // 这样写，会报错。类型“T”上不存在属性“length”。
  return arg
}
```
因为，T 是可以代表任何类型的，编译器不知道类型T上有没有 length 这个属性。
既然我们已经明确知道要传入的是一个数组了，我们可以这样声明Array<T>
```typescript
function getArrayLength<T>(arg: Array<T>) {
  console.log((arg as Array<any>).length) // ok
  return arg
}
```
这就是所谓泛型变量 T 当做类型的一部分使用。

#### 4）泛型接口
```typescript
interface ReturnItemFn<T> {
    (para: T): T
}

const returnItem: ReturnItemFn<number> = para => para
```

#### 5）泛型类
```typescript
class Stack<T> {
    private arr: T[] = []

    public push(item: T) {
        this.arr.push(item)
    }

    public pop() {
        this.arr.pop()
    }
}
```

#### 6）泛型约束
用 <T extends xx> 的方式约束传入泛型的类型:
例如，已知传入的属性number和string其中之一
```typescript
type Params = number | string

class Stack<T extends Params> {
    private arr: T[] = []

    public push(item: T) {
        this.arr.push(item)
    }

    public pop() {
        this.arr.pop()
    }
}
```

#### 7）泛型约束与索引类型
假设我们获取传入对象的某个key的值：
```typescript
function getValue(obj: object, key: string) {
  return obj[key] // error
}
```
正确的写法应该是：
```typescript
function getValue<T extends object, U extends keyof T>(obj: T, key: U) {
  return obj[key] // ok
}
```
首先我们需要约束第一个参数的泛型类型为o'b'ject。这主要是给第二个参数准备的。
因为第二个参数 key 是不是存在于 obj 上是无法确定的，因此我们需要对这个 key 也进行约束，我们把它约束为只存在于 obj 属性的类型，这个时候需要借助到后面我们会进行学习的索引类型进行实现 <U extends keyof T>，我们用索引类型 keyof T 把传入的对象的属性类型取出生成一个联合类型，这里的泛型 U 被约束在这个联合类型中，这样一来函数就被完整定义了。

#### 8）使用多重类型进行泛型约束
上述都是单一类型对泛型的约束方式，多重类型进行泛型约束如何做呢？
举个栗子：
某个泛型只被允许实现以下两个接口的类型：
```typescript
interface FirstInterface {
  doSomething(): number
}

interface SecondInterface {
  doSomethingElse(): string
}

// 我们只能将接口 FirstInterface 与 SecondInterface 作为超接口来解决问题
interface ChildInterface extends FirstInterface, SecondInterface {

}

// 这个时候 ChildInterface 是 FirstInterface 与 SecondInterface 的子接口，然后我们通过泛型约束就可以达到多类型约束的目的。
class Demo<T extends ChildInterface> {
  private genericProperty: T

  useT() {
    this.genericProperty.doSomething()
    this.genericProperty.doSomethingElse()
  }
}
```

#### 9）泛型与new
我们假设需要声明一个泛型拥有构造函数，比如：
```typescript
function factory<T>(type: T): T {
  return new type() // This expression is not constructable.
}
```
编译器会告诉我们这个表达式不能构造，因为我们没有声明这个泛型 T 是构造函数，这个时候就需要 new 的帮助了。
```typescript
function factory<T>(type: {new(): T}): T {
  return new type() // ok
}
```
参数 type 的类型 {new(): T} 就表示此泛型 T 是可被构造的，在被实例化后的类型是泛型 T。