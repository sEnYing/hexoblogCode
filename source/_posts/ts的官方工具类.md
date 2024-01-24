---
title: ts的官方工具类
date: 2023-09-26 16:57:16
description: ts中常用的内置方法
tags:
- typescript
categories:
- typescript
---

<h1>
Partial
</h1>
<p>
Partial 工具类型可以将一个类型的所有属性变为可选的，且该工具类型返回的类型是给定类型的所有子集
</p>

```ts
type Partial<T> = {
  [P in keyof T]?: T[P];
};
interface Person {
  name: string;
  age?: number;
  weight?: number;
}
type PartialPerson = Partial<Person>;
// 相当于
interface PartialPerson {
  name?: string;
  age?: number;
  weight?: number;
}
```

<h1>
Required
</h1>
<p>
与 Partial 工具类型相反，Required 工具类型可以将给定类型的所有属性变为必填的
</p>

```ts
type Required<T> = {
  [P in keyof T]-?: T[P];
};
type RequiredPerson = Required<Person>;
// 相当于
interface RequiredPerson {
  name: string;
  age: number;
  weight: number;
}
```

<h1>
Readonly
</h1>
<p>
Readonly 工具类型可以将给定类型的所有属性设为只读，这意味着给定类型的属性不可以被重新赋值
</p>

```ts
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
type ReadonlyPerson = Readonly<Person>;
// 相当于
interface ReadonlyPerson {
  readonly name: string;
  readonly age?: number;
  readonly weight?: number;
}
```

<h1>
Pick
</h1>
<p>
Pick 工具类型可以从给定的类型中选取出指定的键值，然后组成一个新的类型
</p>

```ts
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};
type NewPerson = Pick<Person, 'name' | 'age'>;
// 相当于
interface NewPerson {
  name: string;
  age?: number;
}
```

<h1>
Omit
</h1>
<p>
与 Pick 类型相反，Omit 工具类型的功能是返回去除指定的键值之后返回的新类型
</p>

```ts
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
type NewPerson = Omit<Person, 'weight'>;
// 相当于
interface NewPerson {
  name: string;
  age?: number;
}
```

<h1>
Exclude
</h1>
<p>
Exclude 的作用就是从联合类型中去除指定的类型
</p>

```ts
type Exclude<T, U> = T extends U ? never : T;
type T = Exclude<'a' | 'b' | 'c', 'a'>; // => 'b' | 'c'
type NewPerson = Omit<Person, 'weight'>;
// 相当于
type NewPerson = Pick<Person, Exclude<keyof Person, 'weight'>>;
// 其中
type ExcludeKeys = Exclude<keyof Person, 'weight'>; // => 'name' | 'age'
```

<h1>
Extract
</h1>
<p>
Extract 类型的作用与 Exclude 正好相反，Extract 主要用来从联合类型中提取指定的类型，类似于操作接口类型中的 Pick 类型。
</p>

```ts
type Extract<T, U> = T extends U ? T : never;
type T = Extract<'a' | 'b' | 'c', 'a'>; // => 'a'
```

<h1>
NonNullable
</h1>
<p>
NonNullable 的作用是从联合类型中去除 null 或者 undefined 的类型
</p>

```ts
type NonNullable<T> = T extends null | undefined ? never : T;
// 等同于使用 Exclude
type NonNullable<T> = Exclude<T, null | undefined>;
type T = NonNullable<string | number | undefined | null>; // => string | number
```

<h1>
Record
</h1>
<p>
Record 的作用是生成接口类型，然后我们使用传入的泛型参数分别作为接口类型的属性和值。
</p>

```ts
type Record<K extends keyof any, T> = {
    [P in K]: T;
};
type MenuKey = 'home' | 'about' | 'more';
interface Menu {
    label: string;
    hidden?: boolean;
}
const menus: Record<MenuKey, Menu> = {
    about: { label: '关于' },
    home: { label: '主页' },
    more: { label: '更多', hidden: true },
};
```

<h1>
ConstructorParameters
</h1>
<p>
ConstructorParameters 可以用来获取构造函数的构造参数，而 ConstructorParameters 类型的实现则需要使用 infer 关键字推断构造参数的类型。

关于 infer 关键字，我们可以把它当成简单的模式匹配来看待。如果真实的参数类型和 infer 匹配的一致，那么就返回匹配到的这个类型。
</p>

```ts
type ConstructorParameters<T extends new (...args: any) => any> = T extends new (
  ...args: infer P
) => any
  ? P
  : never;
class Person {
  constructor(name: string, age?: number) {}
}
type T = ConstructorParameters<typeof Person>; // [name: string, age?: number]
```

<h1>
Parameters
</h1>
<p>
Parameters 的作用与 ConstructorParameters 类似，Parameters 可以用来获取函数的参数并返回序对。
</p>

```ts
type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never;
type T0 = Parameters<() => void>; // []
type T1 = Parameters<(x: number, y?: string) => void>; // [x: number, y?: string]
```

<h1>
ReturnType
</h1>
<p>
ReturnType 的作用是用来获取函数的返回类型。
</p>

```ts
type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;
type T0 = ReturnType<() => void>; // => void
type T1 = ReturnType<() => string>; // => string
```

<h1>
ThisParameterType
</h1>
<p>
ThisParameterType 可以用来获取函数的 this 参数类型。
</p>

```ts
type ThisParameterType<T> = T extends (this: infer U, ...args: any[]) => any ? U : unknown;
type T = ThisParameterType<(this: Number, x: number) => void>; // Number
```

<h1>
ThisType
</h1>
<p>
ThisType 的作用是可以在对象字面量中指定 this 的类型。ThisType 不返回转换后的类型，而是通过 ThisType 的泛型参数指定 this 的类型。
</p>

```ts
type ObjectDescriptor<D, M> = {
    data?: D;
    methods?: M & ThisType<D & M>; // methods 中 this 的类型是 D & M
};
function makeObject<D, M>(desc: ObjectDescriptor<D, M>): D & M {
    let data: object = desc.data || {};
    let methods: object = desc.methods || {};
    return { ...data, ...methods } as D & M;
}
const obj = makeObject({
    data: { x: 0, y: 0 },
    methods: {
        moveBy(dx: number, dy: number) {
            this.x += dx; // this => D & M
            this.y += dy; // this => D & M
        },
    },
});
obj.x = 10;
obj.y = 20;
obj.moveBy(5, 5);
```

<h1>
OmitThisParameter
</h1>
<p>
OmitThisParameter 工具类型主要用来去除函数类型中的 this 类型。如果传入的函数类型没有显式声明 this 类型，那么返回的仍是原来的函数类型。
</p>

```ts
type OmitThisParameter<T> = unknown extends ThisParameterType<T>
    ? T
    : T extends (...args: infer A) => infer R
        ? (...args: A) => R
        : T;
type T = OmitThisParameter<(this: Number, x: number) => string>; // (x: number) => string
```
