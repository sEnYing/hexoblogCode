---
title: javascript函数相关
date: 2023-11-02 16:07:03
description: js的纯函数/柯里化/函数组合
tags:
  - javascript
categories:
  - javascript
---

## 纯函数

#### 说明：相同的输入永远会得到相同的输出，而且没有任何可观察的副作用。副作用让一个函数变的不纯，纯函数的根据相同的输入返回相同的输出，如果函数依赖于外部的状态就无法保证输出相同，就会带来副作用。

## 柯里化

#### 说明：当一个函数有多个参数的时候先传递一部分参数调用它（这部分参数以后永远不变），然后返回一个新的函数接收剩余的参数，返回结果

## lodash中的柯里化函数

#### _.curry(func)

#### 功能：创建一个函数，该函数接收一个或多个 func 的参数，如果 func 所需要的参数都被提供则执行 func 并返回执行的结果。否则继续返回该函数并等待接收剩余的参数。

#### 参数：需要柯里化的函数

#### 返回值：柯里化后的函数

```javascript
const _ = require('lodash')

// 要柯里化的函数
function getSum(a, b, c) {
    return a + b + c
}

// 柯里化后的函数
let curried = _.curry(getSum)
// 测试
curried(1, 2, 3)
curried(1)(2)(3)
curried(1, 2)(3)
```

```javascript
const _ = require('lodash')
const match = _.curry(function (reg, str) {
    return str.match(reg)
})
const haveSpace = match(/\s+/g)
const haveNumber = match(/\d+/g)
console.log(haveSpace('hello world'))
console.log(haveNumber('25$'))
const filter = _.curry(function (func, array) {
    return array.filter(func)
})
console.log(filter(haveSpace, ['John Connor', 'John_Donne']))
const findSpace = filter(haveSpace)
console.log(findSpace(['John Connor', 'John_Donne']))
```

### 模拟 _.curry() 的实现

```javascript
function curry(func) {
    return function curriedFn(...args) {
        // 判断实参和形参的个数
        if (args.length < func.length) {
            return function () {
                return curriedFn(...args.concat(Array.from(arguments)))
            }
        }
        // 实参和形参个数相同，调用 func，返回结果
        return func(...args)
    }
}
```

## 函数组合(compose)

#### 说明：函数组合可以让我们把细粒度的函数重新组合生成一个新的函数。如果一个函数要经过多个函数处理才能得到最终值，这个时候可以把中间过程的函数合并成一个函数

#### 注：函数组合默认是从右到左执行

```javascript
// 组合函数
function compose(f, g) {
    return function (x) {
        return f(g(x))
    }
}

function first(arr) {
    return arr[0]
}

function reverse(arr) {
    return arr.reverse()
}

// 从右到左运行
let last = compose(first, reverse)
console.log(last([1, 2, 3, 4]))
```

### lodash 中的组合函数

#### 说明：lodash 中组合函数 flow() 或者 flowRight()，他们都可以组合多个函数

#### 注：flow() 是从左到右运行，flowRight() 是从右到左运行，使用的更多一些

```javascript
const _ = require('lodash')
const toUpper = s => s.toUpperCase()
const reverse = arr => arr.reverse()
const first = arr => arr[0]
const f = _.flowRight(toUpper, first, reverse)
console.log(f(['one', 'two', 'three']))
```

#### 模拟实现 lodash 的 flowRight 方法

```javascript
// 多函数组合
function compose(...fns) {
    return function (value) {
        return fns.reverse().reduce(function (acc, fn) {
            return fn(acc)
        }, value)
    }
}

// ES6
const compose = (...fns) => value => fns.reverse().reduce((acc, fn) =>
    fn(acc), value)
```

#### 函数的组合要满足结合律 (associativity)

```javascript
// 结合律（associativity）
let f = compose(f, g, h)
let associative = compose(compose(f, g), h) == compose(f, compose(g, h))
//
```

#### 调试组合函数

```javascript
const _ = require('lodash')
const trace = _.curry((tag, v) => {
    console.log(tag, v)
    return v
})
const split = _.curry((sep, str) => _.split(str, sep))
const join = _.curry((sep, array) => _.join(array, sep))
const map = _.curry((fn, array) => _.map(array, fn))
const f = _.flowRight(join('-'), trace('map 之后'), map(_.toLower),
    trace('split 之后'), split(' '))
console.log(f('NEVER SAY DIE'))
```

### lodash/fp

#### lodash 的 fp 模块提供了实用的对函数式编程友好的方法,提供了不可变 auto-curried iteratee-first data-last 的方法

```javascript
// lodash 模块
const _ = require('lodash')
_.map(['a', 'b', 'c'], _.toUpper)
// => ['A', 'B', 'C']
_.map(['a', 'b', 'c'])
// => ['a', 'b', 'c']
_.split('Hello World', ' ')
// lodash/fp 模块
const fp = require('lodash/fp')
fp.map(fp.toUpper, ['a', 'b', 'c'])
fp.map(fp.toUpper)(['a', 'b', 'c'])
fp.split(' ', 'Hello World')
fp.split(' ')('Hello World')
```

```javascript
const fp = require('lodash/fp')
const f = fp.flowRight(fp.join('-'), fp.map(_.toLower), fp.split(' '))
console.log(f('NEVER SAY DIE'))
```
