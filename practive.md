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
