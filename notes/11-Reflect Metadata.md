# Reflect Metadata
> Reflect Metadata 属于 ES7 的一个提案,它的主要作用就是在声明的时候「添加和读取元数据」。
>
> 实际上 Reflect Metadata 的作用就是==附加元数据==，这一点跟 Java 中的注解非常相似。

## 概述
### 前置条件

```
npm i reflect-metadata
# 需要同时在 tsconfig.json 中配置 emitDecoratorMetadata

# TypeScript 的运行时库，包含所有 TypeScript 辅助函数。
npm install tslib
# 需要同时在 tsconfig.json 中配置 importHelpers
```

### 一个例子🌰

```
import 'reflect-metadata';

@Reflect.metadata('name', 'A')
class A {
    @Reflect.metadata('time', '2021.8.31')
    public hello(): string{
        return 'hello world'
    }
}

console.log(Reflect.getMetadata('name', A)); // 'A'
console.log(Reflect.getMetadata('time', new A, 'hello')); // '2021.8.31'
```

## API
```
// define metadata on an object or property
Reflect.defineMetadata(metadataKey, metadataValue, target);
Reflect.defineMetadata(metadataKey, metadataValue, target, propertyKey);
// check for presence of a metadata key on the prototype chain of an object or property
let result = Reflect.hasMetadata(metadataKey, target);
let result = Reflect.hasMetadata(metadataKey, target, propertyKey);
// check for presence of an own metadata key of an object or property
let result = Reflect.hasOwnMetadata(metadataKey, target);
let result = Reflect.hasOwnMetadata(metadataKey, target, propertyKey);
// get metadata value of a metadata key on the prototype chain of an object or property
let result = Reflect.getMetadata(metadataKey, target);
let result = Reflect.getMetadata(metadataKey, target, propertyKey);
// get metadata value of an own metadata key of an object or property
let result = Reflect.getOwnMetadata(metadataKey, target);
let result = Reflect.getOwnMetadata(metadataKey, target, propertyKey);
// get all metadata keys on the prototype chain of an object or property
let result = Reflect.getMetadataKeys(target);
let result = Reflect.getMetadataKeys(target, propertyKey);
// get all own metadata keys of an object or property
let result = Reflect.getOwnMetadataKeys(target);
let result = Reflect.getOwnMetadataKeys(target, propertyKey);
// delete metadata from an object or property
let result = Reflect.deleteMetadata(metadataKey, target);
let result = Reflect.deleteMetadata(metadataKey, target, propertyKey);
// apply metadata via a decorator to a constructor
@Reflect.metadata(metadataKey, metadataValue)
class C {
  // apply metadata via a decorator to a method (property)
  @Reflect.metadata(metadataKey, metadataValue)
  method() {
  }
}
```
四个参数：
- Metadata Key: 元数据的Key，本质上内部实现是一个Map对象，以键值对的形式储存元数据
- Metadata Value: 元数据的Value
- Target: 一个对象，表示元数据被添加在的对象上
- Property: 对象的属性，元数据不仅仅可以被添加在对象上，也可以作用于属性，这跟装饰器类似

## Typescript 内置元数据
```
// 1. design.type - 获取目标的类型
Reflect.getMetadata('design:type', new A, 'hello')
// [Function: Function]

// 2. design:paramtypes - 获取目标参数的类型
Reflect.getMetadata('design:paramtypes', new A, 'hello')
// []

// 3. design:returntype - 获取方法返回类型的信息
Reflect.getMetadata('design:returntype', new A, 'hello')
// [Function: String]
```

## 实践
在 Node.js 中有一些框架，比如 Nestjs 会有分散式的装饰器路由，比如 @Get @Post 等，正是借助 Reflect Metadata 实现的。
比如一个博客系统的文章路由，可能会是下面的代码:
```
@Controller('/article')
class Home {
    @Get('/content')
    someGetMethod() {
      return 'hello world';
    }
  
    @Post('/comment')
    somePostMethod() {}
}
```

Angular 或者 Nestjs 这样的框架就是通过 Decorator + Reflect.metadata 的组合来模拟注解(Annotation)的功能，上面的路由实践环节就是如此。