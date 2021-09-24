# TypeScript数据类型

## 1 **原始类型**

```typescript
boolean, number, string, void, undefined, null, symbol, bigint
```

注意⚠️：首字母都是小写。

## 2 **其他常见类型**

### 2.1 比如计算机类型系统理论中的[顶级类型](https://en.wikipedia.org/wiki/Top_type):

- any
- unknown -- unknown 是 TypeScript 3.0 引入了新类型,是 any 类型对应的安全类型。

### 2.2 比如类型系统中的[底部类型](https://en.wikipedia.org/wiki/Bottom_type):

- never  -- 永不存在的类型，常用的使用场景：

```typescript
// 抛出异常的函数永远不会有返回值
function error(message: string): never {
    throw new Error(message);
}
// 空数组，而且永远是空的
const empty: never[] = []
```

### 2.3 再比如非原始类型(non-primitive type):

- object

当然还有比较常见的数组、元组等等。**普通对象、枚举、数组、元组通通都是 object 类型**。

数组:

```typescript
// 使用泛型
const list: Array<number> = [1, 2, 3]
// 元素类型后面接上[]
const list: number[] = [1, 2, 3]
```

元组--可以看成严格版的数组:

```typescript
let x: [string, number];
x = ['hello', 10, false] // Error
x = ['hello'] // Error
```

