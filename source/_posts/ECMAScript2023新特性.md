---
title: ECMAScript2023新特性
date: 2024-03-07 09:27:51
description: ES14新特性，记录常看常用
tags:
  - javascript
categories:
  - javascript
---

### 新数组方法

##### toReversed/toSpliced/toSorted，原方法的非破坏性替代方法

1. toReversed

```typescript
const array = [1, 2, 3];
const array2 = array.toReversed();
console.log(array2); // [3, 2, 1]
console.log(array); // [1, 2, 3]

```

2. toSorted

```typescript
const array = [1, 3, 7, 11, 6, 9];
const sortedArray = array.toSorted((a, b) => a - b);
console.log(sortedArray); // [1, 3, 6, 7, 9, 11]
console.log(array); // [1, 3, 7, 11, 6, 9]
```

3. toSpliced

```typescript
const array = [1, 3, 7, 11, 6, 9];
const splicedArray = array.toSpliced(1, 2);
console.log(splicedArray); // [1, 11, 6, 9]
console.log(array); // [1, 3, 7, 11, 6, 9]
```

##### findLast/findLastIndex 数组从后查询方法

```typescript
const array = [1, 2, 3, 4, 5];

const lastElement = array.findLast((num) => num < 2);
console.log(lastElement); // 1

const lastIndex = array.findLastIndex((num) => num < 2);
console.log(lastIndex); // 0

const lastElement2 = array.findLast((num) => num < 1);
console.log(lastElement2); // undefined

const lastIndex2 = array.findLastIndex((num) => num < 1);
console.log(lastIndex2); // -1

```

### with方法，替换原数组的对应值

```typescript
const arr = [1, 2, 3, 4, 5];
console.log(arr.with(2, 6)); // [1, 2, 6, 4, 5]
console.log(arr); // [1, 2, 3, 4, 5]

const arr2 = [1, , 3, 4, , 6];
console.log(arr2.with(0, 2)); // [2, undefined, 3, 4, undefined, 6]
```

### 使用Symbols 作为 WeakMap 键

```typescript
const key1 = Symbol("key1");
const key2 = Symbol("key2");

const map = new WeakMap();
map.set(key1, "hexo");

console.log(map.get(key1)); // "hexo"
console.log(map.get(key2)); // undefined
```

### Hashbang

```typescript
#!/usr/bin/env node

console.log("hexo");
```

### 新类型 Record/Tuple

```typescript
const person = {
    name: "sey",
    age: 29,
    day: ['eat', 'sleep', 'work']
};
const animal = ['cat', 'dog', 'chicken', 'duck']
```

### 管道运算符

```typescript
const arr = [1, 2, 3, 4, 5]
const func1 = (...argument) => {
    return 1
}
const func2 = (...argument) => {
    return 2
}
const func3 = (...argument) => {
    return 3
}
const result = arr |> func1() |> func2() |> func3()
```

### 异步迭代器

```typescript
// 迭代器
var num = 0

function* iten(d1, d2, d3, d4) {
    yield d1 + d2 + num
    yield d2 + d3 + num
    yield d3 + d4 + num
}

const itenFunc = iten(2, 4, 6, 8)
const itenResult1 = itenFunc.next()
console.log(itenResult1) // {value: 6, done: false}
num += itenResult1.value
console.log(num) // 6
const itenResult2 = itenFunc.next()
console.log(itenResult2) // {value: 16, done: false}
num += itenResult2.value
console.log(num) // 22
const itenResult3 = itenFunc.next()
console.log(itenResult3) // {value: 36, done: true}
num += itenResult3.value
console.log(num) // 58
const itenResult4 = itenFunc.next()
console.log(itenResult4) // {value: undefined, done: true}
```

```typescript
// 异步迭代器
var num = 0

async function* iten(d1, d2, d3, d4) {
    yield d1 + d2 + num
    yield d2 + d3 + num
    yield d3 + d4 + num
}

const iterator = iten(2, 4, 6, 8);

for await (const number of iterator) {
    num += number
    console.log('number', number);
    console.log('num', num);
    // number 6
    // num 6
    // number 16
    // num 22
    // number 36
    // num 58
    // undefined
}
```