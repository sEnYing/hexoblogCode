---
title: js设计模式
date: 2023-11-01 15:53:12
description: 前端js总计23种设计模式简析
tags:
  - javascript
  - 设计模式
categories:
  - javascript
  - 设计模式
---

<h2>设计模式三大分类</h2>
<ol>
    <li>创建型模式：用于描述对象的创建方式，包括单例模式、工厂模式、建造者模式、原型模式等</li>
    <li>结构型模式：用于描述对象之间的组合方式，包括适配器模式、桥接模式、装饰者模式、外观模式、享元模式、组合模式等。</li>
    <li>行为型模式：用于描述对象之间的交互方式，包括观察者模式、命令模式、迭代器模式、模板方法模式、策略模式、职责链模式、状态模式、访问者模式、中介者模式等。</li>
</ol>
<h3>1.单例模式</h3>
<p>保证一个类仅有一个实例，并提供一个全局访问该实例的接口。</p>

```javascript
class People {
    static instance = null

    constructor(name) {
        if (Singleton.instance !== null) {
            return Singleton.instance
        }
        this.name = name
        Singleton.instance = this
    }

    say() {
        console.log(`我的名字是${this.name}`)
    }
}

// 创建实例
const xiaoming = new People('小明')
xiaoming.say()  // 我的名字是小明

const xiaohong = new People('小红')
xiaohong.say()  // 我的名字是小明
```

<h3>2.工厂模式</h3>
<p>工厂模式是一种创建对象的方式，其不直接调用构造函数来创建对象，而是提供一个共同的接口来创建对象。</p>

```javascript
class Man {
    constructor(name) {
        this.name = name
    }

    init() {
        console.log('man')
    }

    say() {
        console.log('我是男生,我的名字是' + name)
    }
}

class Woman {
    constructor(name) {
        this.name = name
    }

    init() {
        console.log('woman')
    }

    say() {
        console.log('我是女生,我的名字是' + name)
    }
}

class Factory {
    create(type, name) {
        switch (type) {
            case 'man':
                return new Man(name)
            case 'woman':
                return new Woman(name)
            default:
                return new Error("我是什么性别")
        }
    }
}

// 工厂创建
let factory = new Factory()
let man = factory.create('man', '龙日天')
man.init()
man.say()
let woman = factory.create('woman', '小甜甜')
woman.init()
woman.say()
```

<h3>3.观察者模式</h3>
<p>当一个对象被修改时，会自动通知它的依赖者的模式</p>

```javascript
class Subject {
    constructor() {
        this.state = 0
        this.observers = []
    }

    getState() {
        return this.state
    }

    setState(state) {
        this.state = state
        this.notify()
    }

    notify() {
        this.observers.forEach(observer => {
            observer.update()
        })
    }

    add(observer) {
        this.observers.push(observer)
    }
}

// 观察者
class Observer {
    constructor(name, subject) {
        this.name = name
        this.subject = subject
        this.subject.add(this)
    }

    update() {
        console.log(`${this.name} update, state: ${this.subject.getState()}`)
    }
}

// 测试
let s = new Subject()
let o1 = new Observer('o1', s)
let o2 = new Observer('02', s)

s.setState(111)
```

<h3>4.命令模式</h3>
<p>将请求封装为一个对象，从而使你可用不同的请求对客户进行参数化；对请求排队或记录请求日志，以及支持可撤销操作的模式。命令模式将请求者和接受者解耦，使得它们不需要接触到具体的实现。</p>

```javascript
// 接收者
class Receiver {
    login() {
        console.log('登录系统')
    }
}

// 命令
class Command {
    constructor(receiver) {
        this.receiver = receiver
    }
}

// 登录命令
class LoginCommand extends Command {
    execute() {
        console.log('执行登录操作')
        this.receiver.login()
    }
}

// 发起者
class Invoker {
    setCommand(command) {
        this.command = command
    }

    execute() {
        console.log('执行登录命令')
        this.command.execute()
    }
}

// 创建接收者
const receiver = new Receiver()

// 创建命令
const command = new LoginCommand(receiver)

// 创建发起者
const invoker = new Invoker()
invoker.setCommand(command)

// 执行命令
invoker.execute()  // 执行登录操作
```

<h3>5.状态模式</h3>
<p>允许一个对象在其内部状态改变的时候改变它的行为，对象看起来似乎修改了它的类</p>

```javascript
// 状态 （上架，下架，成交）
class State {
    constructor(state) {
        this.state = state
    }

    handle(context) {
        console.log(`this is ${this.state} state`)
        context.setState(this)
    }
}

class Context {
    constructor() {
        this.state = null
    }

    getState() {
        return this.state
    }

    setState(state) {
        this.state = state
    }
}

// test 
let context = new Context()
let up = new State('up')
let down = new State('down')
let ok = new State('ok')

// 上架
up.handle(context)
console.log(context.getState())

// 下架
down.handle(context)
console.log(context.getState())

// 成交
ok.handle(context)
console.log(context.getState())
```

<h3>6.迭代器模式</h3>
<p>提供一种方法顺序一个聚合对象中各个元素，而又不暴露该对象的内部表示。</p>

```javascript
class Iterator {
    constructor(conatiner) {
        this.list = conatiner.list
        this.index = 0
    }

    next() {
        if (this.hasNext()) {
            return this.list[this.index++]
        }
        return null
    }

    hasNext() {
        if (this.index >= this.list.length) {
            return false
        }
        return true
    }
}

class Container {
    constructor(list) {
        this.list = list
    }

    getIterator() {
        return new Iterator(this)
    }
}

// 测试代码
let container = new Container([1, 2, 3, 4, 5])
let iterator = container.getIterator()
while (iterator.hasNext()) {
    console.log(iterator.next())
}
```

<h3>7.桥接模式</h3>
<p>将抽象部分与它的实现部分分离，使它们都可以独立地变化</p>

```javascript
class Color {
    constructor(state) {
        this.state = state
    }
}

class Shape {
    constructor(shape, color) {
        this.shape = shape
        this.color = color
    }

    desc() {
        console.log(`${this.color.state} ${this.shape}`)
    }
}

//测试
let red = new Color('red')
let yellow = new Color('yellow')
let circle = new Shape('circle', red)
circle.desc()
let triangle = new Shape('triangle', yellow)
triangle.desc()
```

<h3>8.组合模式</h3>
<p>将对象组合成树形结构，以表示“整体-部分”的层次结构。
通过对象的多态表现，使得用户对单个对象和组合对象的使用具有一致性。</p>

```javascript
// 文件夹类
class Folder {
    constructor(name, children) {
        this.name = name
        this.children = children
    }

    // 在文件夹下增加文件或文件夹
    add(...fileOrFolder) {
        this.children.push(...fileOrFolder)
        return this
    }

    // 扫描方法
    scan(cb) {
        this.children.forEach(child => child.scan(cb))
    }
}

// 文件类
class File {
    constructor(name, size) {
        this.name = name
        this.size = size
    }

    // 在文件下增加文件，应报错
    add(...fileOrFolder) {
        throw new Error('文件下面不能再添加文件')
    }

    // 执行扫描方法
    scan(cb) {
        cb(this)
    }
}

const foldMovies = new Folder('电影', [
    new Folder('漫威英雄电影', [
        new File('钢铁侠.mp4', 1.9),
        new File('蜘蛛侠.mp4', 2.1),
        new File('金刚狼.mp4', 2.3),
        new File('黑寡妇.mp4', 1.9),
        new File('美国队长.mp4', 1.4)]),
    new Folder('DC英雄电影', [
        new File('蝙蝠侠.mp4', 2.4),
        new File('超人.mp4', 1.6)])
])

console.log('size 大于2G的文件有：')

foldMovies.scan(item => {
    if (item.size > 2) {
        console.log(`name:${item.name} size:${item.size}GB`)
    }
})

// size 大于2G的文件有：
// name:蜘蛛侠.mp4 size:2.1GB
// name:金刚狼.mp4 size:2.3GB
// name:蝙蝠侠.mp4 size:2.4GB
```

<h3>9.原型模式</h3>
<p>用原型实例指向创建对象的种类，并且通过拷贝这些原型创建新的对象</p>

```javascript
class Person {
    constructor(name) {
        this.name = name
    }

    getName() {
        return this.name
    }
}

class Student extends Person {
    constructor(name) {
        super(name)
    }

    sayHello() {
        console.log(`Hello， My name is ${this.name}`)
    }
}

let student = new Student("xiaoming")
student.sayHello()
```

<h3>10.策略模式</h3>
<p>定义一系列的算法，把它们一个个封装起来，并且使它们可以互相替换</p>

```html

<html>
<head>
    <title>策略模式-校验表单</title>
    <meta content="text/html; charset=utf-8" http-equiv="Content-Type">
</head>
<body>
<form id="registerForm" method="post" action="http://xxxx.com/api/register">
    用户名：<input type="text" name="userName">
    密码：<input type="text" name="password">
    手机号码：<input type="text" name="phoneNumber">
    <button type="submit">提交</button>
</form>
<script type="text/javascript">
    // 策略对象
    const strategies = {
        isNoEmpty: function (value, errorMsg) {
            if (value === '') {
                return errorMsg;
            }
        },
        isNoSpace: function (value, errorMsg) {
            if (value.trim() === '') {
                return errorMsg;
            }
        },
        minLength: function (value, length, errorMsg) {
            if (value.trim().length < length) {
                return errorMsg;
            }
        },
        maxLength: function (value, length, errorMsg) {
            if (value.length > length) {
                return errorMsg;
            }
        },
        isMobile: function (value, errorMsg) {
            if (!/^(13[0-9]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|17[7]|18[0|1|2|3|5|6|7|8|9])\d{8}$/.test(value)) {
                return errorMsg;
            }
        }
    }

    // 验证类
    class Validator {
        constructor() {
            this.cache = []
        }

        add(dom, rules) {
            for (let i = 0, rule; rule = rules[i++];) {
                let strategyAry = rule.strategy.split(':')
                let errorMsg = rule.errorMsg
                this.cache.push(() => {
                    let strategy = strategyAry.shift()
                    strategyAry.unshift(dom.value)
                    strategyAry.push(errorMsg)
                    return strategies[strategy].apply(dom, strategyAry)
                })
            }
        }

        start() {
            for (let i = 0, validatorFunc; validatorFunc = this.cache[i++];) {
                let errorMsg = validatorFunc()
                if (errorMsg) {
                    return errorMsg
                }
            }
        }
    }

    // 调用代码
    let registerForm = document.getElementById('registerForm')

    let validataFunc = function () {
        let validator = new Validator()
        validator.add(registerForm.userName, [{
            strategy: 'isNoEmpty',
            errorMsg: '用户名不可为空'
        }, {
            strategy: 'isNoSpace',
            errorMsg: '不允许以空白字符命名'
        }, {
            strategy: 'minLength:2',
            errorMsg: '用户名长度不能小于2位'
        }])
        validator.add(registerForm.password, [{
            strategy: 'minLength:6',
            errorMsg: '密码长度不能小于6位'
        }])
        validator.add(registerForm.phoneNumber, [{
            strategy: 'isMobile',
            errorMsg: '请输入正确的手机号码格式'
        }])
        return validator.start()
    }

    registerForm.onsubmit = function () {
        let errorMsg = validataFunc()
        if (errorMsg) {
            alert(errorMsg)
            return false
        }
    }
</script>
</body>
</html>
```

<h3>10.职责链模式</h3>
<p>多个对象都有机会处理请求，从而避免请求的发送者和接受者之间的耦合关系，将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止</p>

```javascript
// 请假审批，需要组长审批、经理审批、总监审批
class Action {
    constructor(name) {
        this.name = name
        this.nextAction = null
    }

    setNextAction(action) {
        this.nextAction = action
    }

    handle() {
        console.log(`${this.name} 审批`)
        if (this.nextAction != null) {
            this.nextAction.handle()
        }
    }
}

let a1 = new Action("组长")
let a2 = new Action("经理")
let a3 = new Action("总监")
a1.setNextAction(a2)
a2.setNextAction(a3)
a1.handle()
```
