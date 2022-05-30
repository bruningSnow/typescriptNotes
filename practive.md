## 练习题集

[题集来源](https://github.com/type-challenges/type-challenges)

### Pick 实现

作用：挑选对象类型中属性

    type MyPick<T, U extends keyof T> = {
       [key in U]: T[key];
     };

### Readonl 实现

作用：将对象类型全部变为只读

    type MyReadonly<T> = {
       readonly [key in keyof T]: T[key];
    };

### 第一个元素

实现一个通用`First<T>`，它接受一个数组 T 并返回它的第一个元素的类型。

    type First<T extends any[]> = T extends [infer F, ...infer rest] ? F : never

#### 扩展 infer

infer 相当于一个类型收集关键词，与 extends 结合使用。

    1、T extends [infer F, ...infer rest]
    当 T 的数据类型和 [x, ...rest] 结构类型符合，则 F 为 T 中第一个数值类型， rest 为 后续数值类型

    2、type first = T extends [infer F, ...infer rest] ? F : never
    当 T 数据类型与 [x, ...rest]符合，则 first 为 类型 F，否则为 never

[文档介绍](https://jkchao.github.io/typescript-book-chinese/tips/infer.html#%E4%BB%8B%E7%BB%8D)

### 元组转换为对象

传入一个元组类型，将这个元组类型转换为对象类型，这个对象类型的键/值都是从元组中遍历出来。

    type TupleToObject<T extends readonly string[]> = {
        [key in T[number]]: key
    }

### 获取元组长度

创建一个通用的 Length，接受一个 readonly 的数组，返回这个数组的长度。

    type Length<T extends readonly any[]> = T["length"];

### Exclude 实现

实现内置的 Exclude <T, U>类型，但不能直接使用它本身。<br>
从联合类型 T 中排除 U 的类型成员，来构造一个新的类型。

    type MyExclude<T, U> = T extends U ? never : T;

#### 扩展

    type test = 'test' | never
    等同于
    type test = 'test'

### Awaited 实现

假如我们有一个 Promise 对象，这个 Promise 对象会返回一个类型。在 TS 中，我们用 Promise 中的 T 来描述这个 Promise 返回的类型。请你实现一个类型，可以获取这个类型。

```
例如：

  type inferType = {
    tt: string;
  };
  type TPromise = Promise<inferType>;
  type tt = Awaited<TPromise>;

  类型tt 等同于 类型inferType
```

    type Awaited<T extends Promise<any>> = T extends Promise<infer U>
    ? U extends Promise<any>
      ? Awaited<U>
      : U
    : T;

#### 注意

- infer 始终搭配 extends 进行使用
- 三元进行类型间的计算

### If 实现

实现一个 IF 类型，它接收一个条件类型 C ，一个判断为真时的返回类型 T ，以及一个判断为假时的返回类型 F。 C 只能是 true 或者 false， T 和 F 可以是任意类型。

```
例如：

  type A = If<true, 'a', 'b'>  // expected to be 'a'
  type B = If<false, 'a', 'b'> // expected to be 'b'
```

    type If<C extends boolean, T, F> = C extends true ? T : F

### Concat 实现

在类型系统里实现 JavaScript 内置的 Array.concat 方法，这个类型接受两个参数，返回的新数组类型应该按照输入参数从左到右的顺序合并为一个新的数组。

```
例如：

  type Result = Concat<[1], [2]> // expected to be [1, 2]
```

    type Concat<T extends any[], U extends any[]> = [...T, ...U];

#### 注意

```
目前发现在 TS 类型系统计算中 ... (扩展符)，可以应用于 数组/元组，对象类型不可使用 ... (扩展符)进行扩展
```

### Equals 实现

在类型系统里实现类型比较，验证入参类型是否相等

```
例如：

  type Result = Equals<any, number> // expected to be false
  type Result_1 = Equals<any, any> // expected to be true
```

    type Equals<X, Y> = (<T>() => T extends X ? 1 : 2) extends <T>() => T extends Y ? 1 : 2
    ? true
    : false;
