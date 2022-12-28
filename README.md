# 简介

## 什么是 TypeScript

> Typed JavaScript at Any Scale
> 添加了类型系统的 JavaScript，适用于任何规模

### TypeScript 的特性

#### 类型定义

从 TypeScript 的名字就可以看出，「类型」是其最核心的特性。
我们知道，JavaScript 是一门非常灵活的编程语言：

- 它没有类型约束，一个变量可能初始化时是字符串，过一会儿又被赋值为数字。
- 由于存在隐式类型转换，有的变量类型很难再运行前就确定。
- 基于原型的面向对象编程，使得原型上的属性或方法可以在运行时被修改。
- 函数是 JavaScript 的一等公民，可以赋值同时也可以当做参数或返回值。

这种灵活性就像一把双双刃剑，一方面使得 JavaScript 蓬勃发展；但是另外一方面也使得代码的质量参差不齐，维护成本普遍较高，运行时错误多。

##### TypeScript 是静态类型

> 类型系统按照「类型检查的时机」来分类，可以分为动态类型和静态类型

动态类型是指在运行时才会进行类型检查，这种语言的类型错误往往会导致运行时报错。
JavaScript 是一门解释性语言，没有编译阶段 （不像 Java 一样，运行前存在编译阶段），因此它是动态类型。例如以下代码会在运行时报错：

    const variable = 1;
    variable();
    // VM22:2 Uncaught TypeError: variable is not a function
    // at <anonymous>:2:5

静态类型是指在编译阶段就能确定每个变量类型，这种语言的类型错误往往会导致语法错误。
TypeScript 在运行前需要先编译成 JavaScript，而在编译阶段就会进行类型检查，所以
**TypeScript 是静态类型**，以下这段 TypeScript 代码在编译阶段就会报错：

    const variable = 1;
    variable();
    // error TS2349: This expression is not callable.
    // Type 'Number' has no call signatures.

你可能会奇怪，这段 TypeScript 代码看上去和 JavaScript 没什么区别。

没错！大部分 JavaScript 代码都只需要经过少量的修改（或者完全不用修改）就变成
TypeScript 代码，这得益于 TypeScript 强大的**类型推论**，即使不去手动声明变量`variable`的类
型，也能在变量**初始化**时自动推论出它是一个`number`类型。

完整 TypeScript 代码如下：

    const variable = 1;
    variable();
    // error TS2349: This expression is not callable.
    // Type 'Number' has no call signatures.

##### TypeScript 是弱类型

类型系统按照「是否允许隐式类型转换」来分类，可以分为强类型和弱类型。

以下这段代码不管是在 JavaScript 中还是在 TypeScript 中都是可以正常运行的，运行时数字`1`会
被隐式类型转换为字符串`'1'`，加号`+`被识别为字符串拼接，所以打印出的结果是字符串`'11'`。

    let num:number = 1;
    console.log(num + '1');
    // 打印结果为 '11'

TypeScript 是完全兼容 JavaScript 的，它不会修改 JavaScript 运行时的特性，所以它们都是
**弱类型**。

对比，Python 是强类型，以下代码会在运行时报错：

    print(1 + '1');
    # TypeError: unsupported operand type(s) for +: 'int' and 'str'

若要修复该错误，需要进行强制类型转换

    print(str(1) + '1');
    # 打印出字符串 '11'

> 强/弱是相对的，Python 在处理整型和浮点型相加时，会将整型隐式的转换为浮点型，但是这
> 并不影响 Python 是强类型的结论，因为大部分情况下 Python 不会进行隐式类型转换。相对
> 而言，JavaScript 和 TypeScript 中不管加号两侧是什么类型变量，它都会通过隐式类型转
> 换计算出一个结果，而不是报错 --- 所以 JavaScript 和 TypeScript 是弱类型。

> 虽然 TypeScript 不限制加号两侧的类型，但是我们可以借助 TypeScript 的类型系统，以及
> Eslint 提供的代码检查功能，来限制加号两侧必须同为数字或者字符串。在一定程度上使 TypeScript
> 更加接近「强类型」，同时，这种限制是可选的。

这样的类型系统体现了 TypeScript 的核心设计理念，在完整兼容保留 JavaScript 运行时行为的基
础上，通过引入**静态类型系统**来提高代码的可维护性，减少可能出现的 BUG。

##### 类型推论的基本规则

我们知道，当没有手动定义相关变量类型时，TypeScript 会利用**类型推论**来，在变量**初始化**时，自
动隐式的赋予变量类型。

具体规则如下：

- 当初始化时，却并没有给变量赋值，则会默认`any`类型
- 当初始化时，并赋予了变量初始值，则会默认初始值类型

示例代码如下：

    let anyVariable; // 类型为 any
    let num = 1;     // 类型为 number
    // 以此类推

#### 适用于任何规模

TypeScript 非常适用于大型项目 --- 这是显而易见的，类型系统可以为大型项目带来更高的可维
护性，以及更少的 bug。

在中小型项目中推行 TypeScript 的最大障碍就是认为使用 TypeScript 需要写额外的代码，降低
开发效率。但事实上，由于存在**类型推论**，大部分类型都不需要手动声明。相反，TypeScript
增强了编辑器（IDE）的功能，包括代码补全、接口提示、跳转到定义、代码重构等，这在很大程度
上提高了开发效率。而且 TypeScript 有近百个**编译选项**，如果认为类型检查过于严格，那么
可以通过修改编辑器选项来降低类型检查的标准。

TypeScript 还可以和 JavaScript 共存。这意味着如果你有一个使用 JavaScript 开发的旧项目，
又想使用 TypeScript 的特性，那么你不需要着急把整个项目都迁移到 TypeScript，你可以使用
TypeScript 编写新文件，然后在后续更迭中逐步迁移旧文件。如果一些 JavaScript 文件的迁移
成本太高，TypeScript 也提供了一个方案，可以让你在不修改 JavaScript 文件的前提下，编写
一个**类型声明文件**，实现旧项目的渐进式迁移。

事实上，就算你从来没有学习过 TypeScript，你也可能已经在不知不觉中使用了 TypeScript --
vscode 编辑器中编写 JavaScript 时，代码补全和接口提示功能就是通过 TypeScript Language
Service 实现的。

![vscode](https://ts.xcatliu.com/assets/what-is-typescript-vscode.png)

一些第三方库原生支持了 TypeScript，在使用时就能获得代码补全，比如 Vue 3.0

![img](https://ts.xcatliu.com/assets/what-is-typescript-vue.png)

有一些第三方库原生不支持 TypeScript，但是可以通过安装社区维护的**类型声明库**（比如通
过运行 `npm install --save-dev @types/react` 来安装 React 的类型声明库）来获得代码补
全能力 ---- 不管是在 JavaScript 项目中还是 TypeScript 中项目中都是支持的：

![img](https://ts.xcatliu.com/assets/what-is-typescript-react.png)

由此可见，TypeScript 的发展已经深入到前端社区的方方面面了，任何规模的项目都或多或少得到了
TypeScript 的支持。

#### 安装 TypeScript

安装全局 TypeScript 命令
`npm install -g typescript`
这样我们就在全局环境中安装 `tsc` 命令，我们便可以在任何地方执行 `tsc` 命令了。

- 检查全局是否已安装成功 TypeScript
  `tsc -v`

- 新建立一个文件夹，并在其对应的终端输入
  `tsc -init`

- 查询相关 tsconfig.js 配置文档进行配置
  [tsconfig.js 相关配置](https://www.tslang.cn/docs/handbook/tsconfig-json.html)

- 开始编译我们写的 .ts 文件了
  `tsc [name].ts`

## 基础类型

> TypeScript 中包含两个关于类型的概念：顶层类型（所有类型都是其子类）、底层类型（是所有类
> 型的子类）。本部分介绍了 TypeScript 中的常用类型和一些基本概念，旨在让大家对 TypeScript
> 有个初步的 理解。具体包括：

- [原始数据类型](https://ts.xcatliu.com/basics/primitive-data-types.html)
- [任意值](https://ts.xcatliu.com/basics/any.html)
- [类型推论](https://ts.xcatliu.com/basics/type-inference.html)
- [联合类型](https://ts.xcatliu.com/basics/union-types.html)
- [对象的类型 --- 接口](https://ts.xcatliu.com/basics/type-of-object-interfaces.html)
- [数组的类型](https://ts.xcatliu.com/basics/type-of-array.html)
- [函数的类型](https://ts.xcatliu.com/basics/type-of-function.html)
- [类型断言](https://ts.xcatliu.com/basics/type-assertion.html)
- [声明文件](https://ts.xcatliu.com/basics/declaration-files.html)
- [内置对象](https://ts.xcatliu.com/basics/built-in-objects.html)

### 原始数据类型

JavaScript 的类型分为两种：原始数据类型（[Primitive data types](https://developer.mozilla.org/zh-CN/docs/Glossary/Primitive)）和对象类型（Object types）。

原始数据类型包括：布尔值、数值、字符串、`null`、`undefined` 以及 ES6 中的 `Symbol、BigInt`。

主要介绍**前五种**原始数据类型。

#### 布尔值

在 TypeScript 中使用 `boolean` 定义布尔值类型：

`let isDone: boolean = false;`

注意，使用**构造函数 Boolean**创造的是对象，并不是**布尔值**。

#### 数值

使用 `number` 定义数值类型：

```
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

编译结果：

```
var decLiteral = 6;
var hexLiteral = 0xf00d;
// ES6 中的二进制表示法
var binaryLiteral = 10;
// ES6 中的八进制表示法
var octalLiteral = 484;
var notANumber = NaN;
var infinityNumber = Infinity;
```

值得注意的是：TypeScript 会把**二进制**和**八进制**数值转化为十进制。

#### 字符串

使用 `string` 定义字符串类型：

```
let myName: string = 'Tom';
let myAge: number = 25;

// 模板字符串
let sentence: string = `Hello, my name is ${myName}.I'll be ${myAge + 1} years old next month.`;
```

编译结果：

```
var myName = 'Tom';
var myAge = 25;
// 模板字符串
var sentence = "Hello, my name is " + myName + ".I'll be " + (myAge + 1) + " years old next month.";
```

值的注意的是：TypeScript 会把**字符串模板**转化为字符串**加号**。

#### undefined 和 null

在 TypeScript 中，可以使用 `null 和 undefined` 来定义这两个原始数据类型：

```
let u: undefined = undefined;
let n: null = null;
```

与 `void` 和 `never` 的区别是，`null 和 undefined` 即使变量也是类型，且二者都是**底层类型**。

```
// 这样不会报错
let num1: number = undefined;
// 这样也不会报错
let num2: number = null;
```

#### 空值

JavaScript 没有空值（viod）的概念，在 TypeScript 中，可以用 `void` 表示没人任何返回值的**函数**：

```
function alertName(): void {
    alert('My name is Tom');
}
```

声明一个 `void` 类型的变量没有任何意义，它只能赋值 `undefined、num`。（这是因为 `undefined`
和 `null` 都是**底层类型**）。
`let unusable: void = undefined;`

#### 不存在的类型 never

在 TypeScript 中代表的是**永远不会发生**的类型，同时也是**底层类型**。
它的用途一般为：

- 一个从来不会有返回值的函数（如：如果函数内含有 `while(true) {}`）；
- 一个总是会抛出错误的函数（如：`function foo() { throw new Error('Not Implemented') }，foo 的返回类型是 never`）；

[参考资料](https://jkchao.github.io/typescript-book-chinese/typings/neverType.html#%E7%94%A8%E4%BE%8B%EF%BC%9A%E8%AF%A6%E7%BB%86%E7%9A%84%E6%A3%80%E6%9F%A5)

### any 与 unknown

any 与 unknown 都是**顶层类型**，但是二者又有根本的区分。

#### any

即是**顶层类型**又是**底层类型**，其在 TypeScript 表示为任意类型，即不接受任何类型检查。

- 可以表示任何类型的值
- 所规定的变量**可以**使用任何方法
- 变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型

#### unknown

类型为**底层类型**，其在 TypeScript 表示为未知类型，但接受任何类型检查。

- 可以表示任何类型的值
- 所规定的变量**不可以**使用任何方法

### 类型推论

> 如果没有明确的指定类型，那么 TypeScript 会依旧照类型推论（Type inference）的规则推断
> 一个类型。

- 当变量定义时，且被初始化，那么就默认推断为初始化值所属类型
- 当变量定义时，且没有被初始化，那么就默认推断为 `any` 类型

### 联合类型

> 联合类型（Union Types）表示取值可以为多种类型中的一种。

- 联合类型使用 `|` 进行类型分隔
- 变量只能赋值联合类型中的某种类型
- 变量只能访问此联合类型的所有类型中公共的属性

### 对象的类型 ---- 接口

> 在 TypeScript 中，我们使用接口（Interface）来定义对象的类型。

#### 什么是接口

在面向对象语言中，接口（Interface）是一个很重要的概念，它是**行为的抽象**，而具体如何行动需要
由类（class）去实现（implement）。

TypeScript 中的接口是一个非常灵活的概念，除了可以用于**对类的一部分行为进行抽象**以外，也常
用于对**对象的形状**进行描述。

#### 简单例子

```
interface Person {
    name: string;
    age: number;
}

let tom:Peron = {
    name: 'tom',
    age: 24
}
```

- 赋值的时候，变量的属性必须和接口的属性保持一致

#### 可选属性

有时，我们希望不要**完全匹配**一个形状，那么可以用可选属性。

```
interface Person {
    name: string;
    age?: number;
}

let tom:Peron = {
    name: 'tom'
}
```

- 依旧**不允许添加为定义**属性

#### 任意属性

有时候我们希望一个接口允许有任意的属性，可以使用如下方式：

```
interface Person {
    name: string;
    age: number;
    [props: string]: string | number;
}

let tom:Person = {
    name: 'tom',
    age: 24,
    gender: 'male'
}
```

我们使用了 `[props: string]` 定义了任意属性名 `string` 类型。

- 一旦定义了任意属性值，那么确定属性和可选属性值的类型必须是任意属性值的子类。

#### 只读属性

有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly` 定义只读属性：

```
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [props: string]: string | number;
}

let tom: Person = {
    id: 1,
    name: 'tom',
}

tom.name = 'jack';
tom.id = 2;
// Cannot assign to 'id' because it is a constant or a read-only property.
```

- 只读属性的约束在于第一次给对象赋值，而不是第一次给只读属性赋值。

### 数组的类型

> 在 TypeScript 中，数组类型有多种定义方式，比较灵活。

#### 「类型 + 方括号」表示法

```
interface IArr = {
    name: string;
}

let arr: IArr[] = []
```

- 数组内元素只能是方括号外部的类型的子集

#### 数组的泛型

> 利用数组构造函数 `Array` 对外部暴露的泛型变量进行定义。

```
interface IArr = {
    name: string;
}

let arr: Array<IArr> = []
```

- 数组内元素只能是 `Aray` 外部传入的类型的子集

#### 接口表示数组

> 利用数组默认键值为从 0 开始递增的 `number` 类型数。

```
interface IArr {
    [key: number]: number;
}

let arr: IArr = []
```

- 数组内元素只能是接口内部所定义的值类型。

### 函数的类型

> [函数是 JavaScript 中的一等公民](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch2.html#%E4%B8%BA%E4%BD%95%E9%92%9F%E7%88%B1%E4%B8%80%E7%AD%89%E5%85%AC%E6%B0%91)

#### 函数定义

在 JavaScript 中，有两种常用的定义函数的方式---**函数声明**及**函数表达式**。

```
// 函数声明
function fn (x, y) {
    return x + y;
}

// 函数表达式
const fn = function(x, y) {
    return x + y;
}
```

#### 函数声明

函数声明的类型定义

```
function fn (x: string, y:string): string {
    return x + y;
}
```

输入参数类型定义 ---- x 为 `string`， y 为 `string`;
函数返回值类型定义 -- `string`;

- 输入过多（过少）的参数是不被允许的

#### 函数表达式

如果我们现在写一个函数表达式的定义，可能会写成：

```
let fn = function(x: string, y: string): string {
    return x + y;
};
```

这是可以通过的，但是这实际上并不是函数表达式定义，而是函数声明定义加上类型推论。
`fn` 等于函数, 同时函数返回值被函数声明定义方式定义为 `string` ,因此变量 `fn` 被类型
推论类型为 `string`。如果我们使用函数表达式定义方式定义，应该以下写法：

```
let fn: (x: string, y: string) => string = function(x, y) {
    return x + y;
};
```

#### 用接口定义函数

> 我们同时也可以使用接口的方式来定义一个函数需要符合的类型。

```
interface IFn {
    (x: string, y: string): string;
}

let fn:IFn = (x, y) => x + y;
```

#### 可选参数

之前所提及的，输入过多（过少）的参数是不被允许的。那么如何定义可选的参数呢？
与接口定义**可选属性**写法一致，使用 `?` 表示可选参数。

```
const fn = (x: string, y?: string): string => x + y;
```

- 可选参数必须要接在必选参数后面

#### 参数默认值

```
const fn = (x: string = '1', y: string): string => x + y;

fn(undefined, '4');
fn(null, '4');
```

- 当参数被赋予默认值后，可以变相看成它已成为可选属性（可以使用 `null, undefined` 进行代替）
- 含有默认值参数没有限制一定需要放在必选参数后面

#### 剩余参数

使用**三点运算符**进行接收剩余参数。

```
const fn = (x: string, ...rest: string[]): string => x;

fn('1', '2', '3')
```

- `rest` 是一个剩余参数所构成的数组

#### 重载

重载允许一个函数接收不同数量或类型的参数时，做出不同的代码提示处理。

```
// 声明部分 【函数声明】
function fun(x: string): string
function fun(x: number): number

// 实现部分
function fun(x: string | number): string | number {
    return x
}

fun(1)
```

- 重载只能针对于**函数声明**方式

### 类型断言

类型断言（Type Assertion）可以用来手动指定一个值类型。

#### 语法

```
 [value] as [type]
```

或

```
<[type]>[value]
```

上述表达式只是作用手动指定类型，表达式返回值依旧是 `[value]`。

- 因为在 ts 中 `<[type]>[value]` 表达式还可能表达的是泛型，因此我们统一 `[value] as [type]`

#### 类型断言常用用法

类型断言的常见用途有以下几种：

```
type IJoin = number | string;

interface IParent {
    name: string
}

interface IChile {
    name: string;
    age: number
}

const parent_example: IParent = {
    name: 'tom',
}

const child_example: IChile = {
    name: 'tom',
    age: 1
}
```

##### 联合类型断言为其中一个类型

由于 TypeScript 不确定一个联合类型变量到底属于那个类型的时候，我们只能访问**共有**的属性及方法。
但是我们可以**类型断言**来手动指定变量属于当前联合类型中的那个类型，同时也就意味着可以访问当前
指定类型的所有属性及方法。

```
// 联合类型断言为其中一个类型
const fn_1: (param: IJoin) => void = (param) => {
    console.log((param as string).length)
};
```

##### 父类断言为子类

由于子类所定义类型的变量可以赋值于父类所定义变量，例如：

```
IChild extends IParent {
    age: number
}
```

所以当函数接收参数类型为父类类型时，我们为了能确定并访问相关子类特有属性，我们就可以用类型断言，手动
指定外部传入参数为某一子类。

```
const fn_3: (param: IParent) => void = (param) => {
    console.log((param as IChile).age)
}
fn_3(child_example)
```

##### 任意类型断言为 any

理想情况下，TypeScript 的类型系统运转良好，每个值的类型都具备而精确。
当我们引用一个在此类型上不存在的属性或方法时，就会报错。
当然绝大多数这种提示是非常有用的，但是有时候当我们非常确定某个值上存在某个属性且相关类型不便修改。
此时我们可以为其手动编写类型或者直接断言为`any` 类型。

```
const fn_4: (param: IParent) => void = (param) => {
    console.log((param as any).age)
}
fn_4(child_example)
```

- 此种做法极可能隐藏潜在的威胁，如果不是真正十分确定且影响范围小，否则就不要使用 `any`
- 一方面不能滥用 `any`，但同时又不能否定其存在的价值，因此我们需要在类型的严格性以及
  开发便捷之间进行掌握权衡。

##### any 断言为任意类型

在日常开发中，我们极可能会遇到需要处理的 `any` 类型变量。这或许来源于未定义类型的第三方包、项目
遗留的烂代码、受到 TypeScript 可能存在的某些场景限制。此时我们就可以进行手动指定某个类型进行
**亡羊补牢**，为时未晚的进行项目可维护性的提升。

```
const fn_5: (param: any) => void = (param) => {
    console.log((param as IChile).age)
}
fn_5(child_example)
```

#### 类型断言的限制

从上我们可知：

- 联合类型可以断言为其中的某个类型
- 父类可以断言为子类
- 任何类型可以断言为 `any` 类型
- `any` 类型可以断言为任何类型

那试问，类型断言是否存在限制呢？是不是任何一个类型都可以断言为另一个任何类型？
当然，答案是否定的。类型断言的限制如下：
具体来说，若 `A` 兼容 `B`，那么 `A` 可以被断言 `B`，`B` 也可以被断言成 `A`。

**值的注意的是**，TypeScript 并不关心类型定义的过程，它只会关注类型最后定义的结果。
这就意味着两个完全分开的定义的类型可能就是**子/父类**关系，如下：

```
interface IParent {
    name: string
}

interface IChile {
    name: string;
    age: number
}
```

以上就等同于

```
interface IParent {
    name: string
}

interface IChile extends IParent {
    age: number
}
```

#### 双重断言

> 简单来说，就是以任意类型 `any` 为桥梁，已达到任何类型都可以断言为任何类型。
> 当然这样的行为是极具危险性的

```
interface IExample_1 {
    name: string
}

interface IExample_2 {
    age: number
}

const example_2: IExample_1 = {
    name: 'tom'
}

const fn_6:(params: IExample_1) => void = (params) => {
    console.log((params as any as IExample_2).age)
}
```

#### 类型断言 vs 类型转换

类型断言只会影响 TypeScript 编译时的类型， 并不会强制转化编译结果后的类型。

```
function toBoolean(something: any): boolean {
    return something as boolean;
}

toBoolean(1);
// 返回值为 1
```

类型转换就是 JS 的类型转换，它不仅会改变 TypeScript 编译时的类型，当然也会强性
转换编译结果后的类型。

```
function toBoolean(something: any): boolean {
    return Boolean(something);
}

toBoolean(1);
// 返回值为 true
```

#### 类型断言 vs 类型声明

类型断言的限制为：若 `A` 兼容 `B`，那么 `A` 可以被断言 `B`，`B` 也可以被断言成 `A`。

```
const fn_7: (param: IChile) => void = (param) => {
    console.log((param as IParent).name)
}
fn_7(parent_example)
```

类型声明的限制为：若 `A` 兼容 `B` 【也就是说 `A` 继承于 `B`】，那么 `A` 类型变量可以赋值
给 `B` 类型变量；但是 `B` 类型变量不可以赋值给 `A` 类型变量。

```
const b: IParent = child_example
// 可以通过

const a: IChild = parent_example
// 报错 parent_example 没有属性 age
```

### 声明文件

> 当使用第三方库时，我们需要引用它们的第三方声明文件，才能获得对应的代码补全、接口提示等功能。

#### 新语法索引

关于声明文件，设计多种新语法，举例如下：

- `declare var` 声明全局变量
- `declare function` 声明全局方法
- `declare class` 声明全局类
- `declare enum` 声明全局枚举类型
- `declare namespace` 声明（含有子属性的）全局对象
- `interface 和 type` 声明全局类型
- `export` 导出变量
- `export namespace` 导出（含有子属性的）对象
- `declare global` 扩展全局变量
- `declare module` 扩展模块

#### 什么是声明文件

通常我们会把声明语句放到单独的文件（`XXX.d.ts`）中，这就是声明文件：

```
// XXX.d.ts
declare var XXX: (selector: string) => any;

// index.ts
XXX('selector');
```

一般来说，ts 会解析项目中所有的 `*.ts` 文件，当然这包括 `*.d.ts` 文件。所以当我们把 `XXX.d.ts` 放入到项目
中后，其他所有 `*.ts` 文件就可以获得 `XXX` 的类型定义。

假如仍然无法解析，那么可以检查下 `tsconfig.ts` 中的 `files`、`include` 和 `exclude` 配置，确保其
包含了相对应的声明文件，`XXX.d.ts`。

#### 书写声明文件

当我们需要书写自己的声明文件时，我们就需要了解如何书写声明文件了。

##### 全局变量

- `declare var` 声明全局变量
- `declare function` 声明全局方法
- `declare class` 声明全局类
- `declare enum` 声明全局枚举类型
- `declare namespace` 声明（含有子属性的）全局对象
- `interface 和 type` 声明全局类型

##### 直接扩展全局变量

当第三方库扩展一个全局变量，但是此时全局变量的类型却没有，这将会导致 ts 编译错误。此时，就需要
扩展全局变量的类型。比如扩展 `String` 类型。

```
interface String {
    haha(): string
}

'example'.haha()
```

通过声明合并，使用 `interface String` 即可给 `String` 添加属性或方法。

当然，同时也可以使用 `declare namespace` 给已有的空间命名添加类型声明，手法类同声明合并。

```
// 最初 命名空间
declare namespace XXX {
    interface EXAMPLE {
        origin: string
    }
}

// 声明 添加 test 属性定义
declare namespace XXX {
    interface EXAMPLE {
        test: string
    }
}
```

值的注意的是，不能重载在命名空间已有的最叶子类型定义

##### 模块插件

如果是需要扩展原有模块的话，我们首先在类型声明文件先引用原有模块，再使用 `declare module` 扩展有有模块。

```
// types/moment-plugin/index.d.ts

import * as moment from 'moment';

declare module 'moment' {
    export function foo(): moment.CalendarKey;
}

// src/index.ts

import * as moment from 'moment';
import 'moment-plugin';

moment.foo();
```

### 内置对象

JavaScript 中有很多[内置对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)，它们可以直接在 TypeScript 中当做好了的类型

内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和
其他环境（比如 DOM）的标准。

#### ECMAScript 的内置对象

ECMAScript 标准提供的内置对象有：

`Boolean`、`Error`、`Date`、`RegExp` 等。

我们可以在 TypeScript 中将变量定义为这些类型：

```
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
```

更多的内置对象，可以查看 [MDN 的文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)。

而他们的定义文件，则在 [TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/main/src/lib)中。

#### DOM 和 BOM 的内置对象

DOM 和 BOM 提供的内置对象有：

`Document`、`HTMLElement`、`Event`、`NodeList` 等。

TypeScript 中会经常用到这些类型：

```
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySeletorAll('div');
document.addEventListener('click', function(e: MouseEvent) {
    // Do something
})
```

它们的定义文件同样在 [TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/main/src/lib)中。

#### TypeScript 核心库的定义文件

[TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/main/src/lib)中定义了所有浏览器环境需要用到的类型，并且是预置在 TypeScript 中的。

当你在使用一些常用的方法的时候，TypeScript 实际上已经帮你做了很多类型判断的工作了，比如：

```
Math.pow(10, '2');

// index.ts(1,14): error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
```

上面的例子中，`Math.pow` 必须接受两个 `number` 类型的参数。事实上 `Math.pow` 的类型定义如下：

```
interface Math {
    /**
     * Returns the value of a base expression taken to a specified power.
     * @param x The base value of the expression.
     * @param y The exponent value of the expression.
     */
    pow(x: number, y: number): number;
}
```

再举一个 DOM 中的例子：

```
document.addEventListener('click', function(e) {
    console.log(e.targetCurrent);
});

// index.ts(2,17): error TS2339: Property 'targetCurrent' does not exist on type 'MouseEvent'.
```

上面的例子中，`addEventListener` 方法是在 TypeScript 核心库中定义的：

```
interface Document extends Node, GlobalEventHandlers, NodeSelector, DocumentEvent {
    addEventListener(type: string, listener: (ev: MouseEvent) => any, useCapture?: boolean): void;
}
```

所以 `e` 被推断成了 `MouseEvent`，而 `MouseEvent` 是没有 `targetCurrent` 属性的，所以报错了。

注意，TypeScript 核心库的定义中不包含 Node.js 部分。

#### 用 TypeScript 写 Node.js

Node.js 不是内置对象的一部分，如果想用 TypeScript 写 Node.js，则需要引入第三方声明文件：

```
npm install @types/node --save-dev
```

#### 参考

- [内置对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)
- [TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/main/src/lib)

## 进阶

> 本部分介绍一些高级的类型与技术，具体内容包括：

- [类型别名](https://ts.xcatliu.com/advanced/type-aliases.html)
- [字符串字面量类型](https://ts.xcatliu.com/advanced/string-literal-types.html)
- [元组](https://ts.xcatliu.com/advanced/tuple.html)
- [枚举](https://ts.xcatliu.com/advanced/enum.html)
- [类](https://ts.xcatliu.com/advanced/class.html)
- [类与接口](https://ts.xcatliu.com/advanced/class-and-interfaces.html)
- [泛型](https://ts.xcatliu.com/advanced/generics.html)
- [声明合并](https://ts.xcatliu.com/advanced/declaration-merging.html)
- [扩展阅读](https://ts.xcatliu.com/advanced/further-reading.html)

### 类型别名

类型别名用来给一个类型起个新名字。

#### 简单的例子

```
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}
```

上例中，我们使用 `type` 创建类型别名。

类型别名常用于联合类型。

#### 参考

- [Advanced Types # Type Aliases](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/)）

### 字符串字面量类型

字符串字面量类型用来约束取值只能是某几个字符串中的一个。

#### 简单的例子

```
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}

handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dblclick'); // 报错，event 不能为 'dblclick'

// index.ts(7,47): error TS2345: Argument of type '"dblclick"' is not assignable to parameter of type 'EventNames'.
```

上例中，我们使用 `type` 定了一个字符串字面量类型 `EventNames`，它只能取三种字符串中的一种。

注意，**类型别名与字符串字面量类型都是使用** `type` **进行定义**。

#### 参考

- [Advanced Types # Type Aliases](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/)）

### 元组

数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象。

元组起源于函数编程语言（如 F#），这些语言中会频繁使用元组。

#### 简单的例子

定义一对值分别为 `string` 和 `number` 的元组：

```
let tom: [string, number] = ['Tom', 25];
```

当赋值或访问一个已知索引的元素时，会得到正确的类型：

```
let tom: [string, number];
tom[0] = 'Tom';
tom[1] = 25;

tom[0].slice(1);
tom[1].toFixed(2);
```

也可以只赋值其中一项：

```
let tom: [string, number];
tom[0] = 'Tom';
```

```
let tom: [string, number];
tom = ['Tom'];

// Property '1' is missing in type '[string]' but required in type '[string, number]'.
```

#### 越界的元素

当添加越界的元素时，它的类型会被限制为元组中每个类型的联合类型：

```
let tom: [string, number];
tom = ['Tom', 25];
tom.push('male');
tom.push(true);

// Argument of type 'true' is not assignable to parameter of type 'string | number'.
```

#### 参考

- [Advanced Types # Type Aliases](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/)）

### 枚举

枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。

#### 简单的例子

枚举使用 `enum` 关键字来定义：

```
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
```

枚举成员会被赋值为从 `0` 开始递增的数字，同时也会对枚举值到枚举名进行反向映射：

```
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 0); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true

console.log(Days[0] === "Sun"); // true
console.log(Days[1] === "Mon"); // true
console.log(Days[2] === "Tue"); // true
console.log(Days[6] === "Sat"); // true
```

事实上，上面的例子会被编译为：

```
var Days;
(function (Days) {
    Days[Days["Sun"] = 0] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```

#### 枚举赋值

对枚举的初始化赋值要分为赋值 `number` 类型还是 `非number` 类型（都存在前面属性值被后面属性值覆盖情况）。

对于 number 类型，当存在属性没有初始化赋值操作时，属性会从最近一个已赋值的属性每次累增 +1。

对于 非 number 类型，赋值操作必须要对所有属性进行赋值操作或者仅只能对最后一个属性进行赋值操作。

#### 参考

- [Advanced Types # Type Aliases](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/)）
