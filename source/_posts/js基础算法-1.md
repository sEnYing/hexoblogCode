---
title: js基础算法-1
date: 2023-09-27 15:57:39
description: 前端js的基础算法内容，栈、队列
tags:
  - javascript
  - 算法
categories:
  - javascript
  - 算法
---

<h1>栈</h1>
<h3>基础栈的实现</h3>

```javascript
class Stack {
    constructor() {
        // 存储栈的数据
        this.data = {}
        // 记录栈的数据个数（相当于数组的 length）
        this.count = 0
    }

    // push() 入栈方法
    push(item) {
        // 方式1：数组方法 push 添加
        // this.data.push(item)
        // 方式2：利用数组长度
        // this.data[this.data.length] = item
        // 方式3：计数方式
        this.data[this.count] = item
        // 入栈后，count 自增
        this.count++
    }

    // pop() 出栈方法
    pop() {
        // 出栈的前提是栈中存在元素，应先行检测
        if (this.isEmpty()) {
            console.log('栈为空！')
            return
        }
        // 移除栈顶数据
        // 方式1：数组方法 pop 移除
        // return this.data.pop()
        // 方式2：计数方式
        const temp = this.data[this.count - 1]
        delete this.data[--this.count]
        return temp
    }

    // isEmpty() 检测栈是否为空
    isEmpty() {
        return this.count === 0
    }

    // top() 用于获取栈顶值
    top() {
        if (this.isEmpty()) {
            console.log('栈为空！')
            return
        }
        return this.data[this.count - 1]
    }

    // size() 获取元素个数
    size() {
        return this.count
    }

    // clear() 清空栈
    clear() {
        this.data = []
        this.count = 0
    }
}

const s = new Stack()
s.push('a')
s.push('b')
s.push('c')
```

<h3>包含min函数的栈</h3>

```javascript
// 在存储数据的栈外，再新建一个栈，用于存储最小值
class MinStack {
    constructor() {
        // stackA 用于存储数据
        this.stackA = []
        this.countA = 0
        // stackB 用于将数据降序存储（栈顶值为最小值）
        this.stackB = []
        this.countB = 0
    }

    // 入栈
    push(item) {
        // stackA 正常入栈
        this.stackA[this.countA++] = item
        // stackB 如果没有数据，直接入栈
        // 如果 item 的值 <= stackB 的最小值，入栈
        if (this.countB === 0 || item <= this.min()) {
            this.stackB[this.countB++] = item
        }
    }

    // 最小值函数
    min() {
        return this.stackB[this.countB - 1]
    }

    // 获取栈顶值
    top() {
        return this.stackA[this.countA - 1]
    }

    // 出栈
    pop() {
        // 先进行 stackB 的检测
        // 如果 stackA 的栈顶值 === stackB 的栈顶值，stackB 出栈
        if (this.top() === this.min()) {
            delete this.stackB[--this.countB]
        }
        // stackA 出栈
        delete this.stackA[--this.countA]
    }
}

const m = new MinStack()
```

<h3>包含min函数的栈-数组方法</h3>

```javascript
class MinStack {
    constructor() {
        this.stack = []
    }

    // 入栈
    push(item) {
        this.stack.push(item)
    }

    // 查看栈顶值
    top() {
        return this.stack[this.stack.length - 1]
    }

    // 实现最小值功能
    min() {
        return Math.min.apply(null, this.stack)
    }

    // 出栈方法
    pop() {
        return this.stack.pop()
    }
}

const m = new MinStack()
```

<h1>队列</h1>
<h3>队列的实现-基于数组</h3>

```javascript
class Queue {
    constructor() {
        // 用于存储队列数据
        this.queue = []
        this.count = 0
    }

    // 入队方法
    enQueue(item) {
        this.queue[this.count++] = item
    }

    // 出队方法
    deQueue() {
        if (this.isEmpty()) {
            return
        }
        // 删除 queue 的第一个元素
        // delete this.queue[0]
        // 利用 shift() 移除数组的第一个元素
        this.count--
        return this.queue.shift()
    }

    isEmpty() {
        return this.count === 0
    }

    // 获取队首元素值
    top() {
        if (this.isEmpty()) {
            return
        }
        return this.queue[0]
    }

    size() {
        return this.count
    }

    clear() {
        // this.queue = []
        this.length = 0
        this.count = 0
    }
}

const q = new Queue()
```

<h3>队列的实现-基于对象</h3>

```javascript
class Queue {
    constructor() {
        this.queue = {}
        this.count = 0
        // 用于记录队首的键
        this.head = 0
    }

    // 入队方法
    enQueue(item) {
        this.queue[this.count++] = item
    }

    // 出队方法
    deQueue() {
        if (this.isEmpty()) {
            return
        }
        const headData = this.queue[this.head]
        delete this.queue[this.head]
        this.head++
        return headData
    }

    length() {
        return this.count - this.head
    }

    isEmpty() {
        return this.length() === 0
    }

    clear() {
        this.queue = {}
        this.count = 0
        this.head = 0
    }
}

const q = new Queue()
```

<h3>双端队列</h3>

```javascript
class Deque {
    constructor() {
        this.queue = {}
        this.count = 0
        this.head = 0
    }

    // 队首添加
    addFront(item) {
        this.queue[--this.head] = item
    }

    // 队尾添加
    addBack(item) {
        this.queue[this.count++] = item
    }

    // 队首删除
    removeFront() {
        if (this.isEmpty()) {
            return
        }
        const headData = this.queue[this.head]
        delete this.queue[this.head++]
        return headData
    }

    // 队尾删除
    removeBack() {
        if (this.isEmpty()) {
            return
        }
        const backData = this.queue[this.count - 1]
        delete this.queue[--this.count]
        // this.count-- 与 上一步 this.count - 1 合并
        return backData
    }

    // 获取队首值
    frontTop() {
        if (this.isEmpty()) {
            return
        }
        return this.queue[this.head]
    }

    // 获取队尾值
    backTop() {
        if (this.isEmpty()) {
            return
        }
        return this.queue[this.count - 1]
    }

    isEmpty() {
        return this.size() === 0
    }

    size() {
        return this.count - this.head
    }
}

const deq = new Deque()
```

<h3>队列的最大值</h3>

```javascript
var MaxQueue = function () {
    // 存储队列数据
    this.queue = {}
    // 双端队列维护最大值（每个阶段的最大值）
    this.deque = {}
    // 准备队列相关的数据
    this.countQ = this.countD = this.headQ = this.headD = 0
};

/** 队尾入队
 * @param {number} value
 * @return {void}
 */
MaxQueue.prototype.push_back = function (value) {
    // 数据在 queue 入队
    this.queue[this.countQ++] = value
    // 检测是否可以将数据添加到双端队列
    //   - 队列不能为空
    //   - value 大于队尾值
    while (!this.isEmptyDeque() && value > this.deque[this.countD - 1]) {
        // 删除当前队尾值
        delete this.deque[--this.countD]
    }
    // 将 value 入队
    this.deque[this.countD++] = value
};

/** 队首出队
 * @return {number}
 */
MaxQueue.prototype.pop_front = function () {
    if (this.isEmptyQueue()) {
        return -1
    }
    // 比较 deque 与 queue 的队首值，如果相同，deque 出队，否则 deque 不操作
    if (this.queue[this.headQ] === this.deque[this.headD]) {
        delete this.deque[this.headD++]
    }
    // 给 queue 出队，并返回
    const frontData = this.queue[this.headQ]
    delete this.queue[this.headQ++]
    return frontData
};

/** 获取队列最大值
 * @return {number}
 */
MaxQueue.prototype.max_value = function () {
    if (this.isEmptyDeque()) {
        return -1
    }
    // 返回 deque 队首值即可
    return this.deque[this.headD]
};

/** 检测队列 deque 是否为空
 *
 */
MaxQueue.prototype.isEmptyDeque = function () {
    return !(this.countD - this.headD)
};

/** 检测队列 Queue 是否为空
 *
 */
MaxQueue.prototype.isEmptyQueue = function () {
    return !(this.countQ - this.headQ)
};
```
