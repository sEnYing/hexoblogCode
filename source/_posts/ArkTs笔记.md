---
title: ArkTs笔记
date: 2024-01-19 09:44:29
description: ArkTs学习过程笔记，写法小技巧以及文档不好找内容
tags:
  - ArkTs
categories:
  - ArkTs
---

### Next版本注意事项

1. 不可用.call,.bind,.apply
2. 不可用解构赋值，拓展运算符
3. 不可用索引形式获取对象值，如 abc = {a: b}, abc['a']不可用
4. 对象数据类型，需定义interface，以及 定义class去 implements该interface
5. 使用字面量赋值前，需提前指定该参数的数据类型
6. 不支持在箭头函数上写泛型
7. 禁止一切any
8. 不能for...in...，只有Object类型可以用 a[b] 的方式取值
9. 函数有返回值的话，要写上返回值类型，不然会报错。Promise也要加泛型如

```typescript
new Promise<string>((resolve) => {
    resolve('111')
})
```

10. 对象修改@state修饰符只能监听到第一层，深层级对象修改值时，直接通过 . 修改，则双向数据绑定不生效，写法为

```typescript
interface IObj_c {
    c_a: number
    c_b: string
}

class Obj_cImpl implements IObj_c {
    c_a: number
    c_b: string

    constructor(Obj_cImpl: IObj_c) {
        this.c_a = Obj_cImpl.c_a
        this.c_b = Obj_cImpl.c_b
    }
}

interface IObj {
    a: number,
    b: string,
    c: IObj_c
}

class ObjImpl implements IObj {
    a: number
    b: string
    c: IObj_c

    constructor(ObjImpl: IObj) {
        this.a = ObjImpl.a
        this.b = ObjImpl.b
        this.c = ObjImpl.c
    }
}

let obj: IObj = new ObjImpl({
    a: 10,
    b: '10',
    c: new Obj_cImpl({
        c_a: 10,
        c_b: '10'
    })

})

obj.c.c_a = 10 // 不生效,需加以下代码
this.obj = new ObjImpl(obj)
```

### Ability功能

###### 1. 应用中拉起新Ability并互相传参

```typescript
// 拉起新Ability传参
// 被拉起的Ability生命周期内，onCreate的want参数中存在parameters为传过来的值，
// 通过AppStorage存入，应用内使用
const context = getContext(this) as common.UIAbilityContext
// 如果不as，类型为context的话无法推导建议
let want: Want = {
    bundleName: '',
    abilityName: '',
    moduleName: '', // 可选
    parameters: {
        key: value
    } //  可选，Ability拉起传参用
// 从module.json5找  name 为 abilityName
// 从 AppScope  app.json5 找 bundleName
// 从根目录build.profile.json5中找到 module为 moduleName
}
context.startAbility(want).then((data) => {
    // 该方法不记返回结果
    console.log(data)
})

context.startAbilityForResult(want).then((data) => {
    // 该方法接受返回结果
    console.log(data)
})

// 返回旧Ability传参
const context2 = getContext(this) as common.UIAbilityContext
context2.terminateSelfWithResult({
    resultCode: 1,
    want: {
        bundleName: '',
        abilityName: '',
        moduleName: '',
        parameters: {
            key: value
        }
    }
})

// 必须 模拟器 debugger调试 可以在UIAbility中 断点调试
```

### DevEco studio功能

###### 1. 运行多模块方法：编辑配置 => Deploy Muti Hap => 勾选Deploy Muti Hap Packages