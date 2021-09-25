# infer 关键字

## 概念理解
`infer` 是工具类型和底层库中常常用到的关键字，==表示在 extends 条件语句中待推断的类型变量==;简单来说， infer 关键字就是声明一个类型变量，当系统给足条件的时候就会被推断出来。

## 应用
1. 设计实现 ReturnType,用于获取函数的返回类型

```
// 参照lib.es5.d.ts(1531, 6)的实现
type ReturnType1<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;
interface User {
    id: number
    name: string
    form?: string
}
type Foo = () => User
type R3 = ReturnType1<Foo> // User
```

2. 设计实现 ConstructorParameters<T> – 提取构造函数中参数类型

```
type ConstructorParameters1<T extends new (...args: any) => any> = T extends new (...args: infer P) => any ? P : never;
// lib.es5.d.ts(1531, 6)的实现
// type ConstructorParameters<T extends abstract new (...args: any) => any> = T extends abstract new (...args: infer P) => any ? P : never;

class TestClass {
    constructor(public name: string, public age: number) {}
}
type R4 = ConstructorParameters<typeof TestClass> //typeof 操作符可以用来获取一个变量或对象的类型。
// 输出：[name: string, age: number]
```

3. tuple转union,比如[string, number] -> string | number

```
type ElementOf<T> = T extends Array<infer P> ? P : never;
type TTuple = [string, number];
type ToUnion = ElementOf<TTuple>; // string | number
```