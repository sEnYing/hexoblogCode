---
title: ArkTs笔记2
date: 2024-01-22 14:04:14
description: ArkTs学习过程笔记，长按事件，及相关展示内容
tags:
  - ArkTs
categories:
  - ArkTs
---

### 长按事件写法

##### 事件在元素后加，如下，abc为某个元素

```typescript
// 长按滑动等统一用该方法 gesture，LongPressGesture为长按，还有其余方法
abc()
    .gesture(
        LongPressGesture().onAction(()=>{
        // 触发长按回调
         this.showAlert = true   
}))
    // 元素上显示对话气泡
    .bindPopup(this.showAlert, {
        placement: Placement.Top,
        // builder的值为 @Builder修饰符生成的元素
        builder: this.getBuilder
    })
```

### canvas绘制图片

```typescript
// 引入图片方法
image: ImageBitmap = new ImageBitmap('/xxx.png')
context: CanvasRenderingContext2D = new CanvasRenderingContext2D(//是否需要抗锯齿 
)
this.context.drawImage(image, x, y, kuan, gao)

this.context.translate(x, y) // 移动圆心方法
this.context.rotate(180 * Math.PI / 180) // 旋转，弧度  Math.PI / 180

this.context.save() // 保存路径
this.context.restore() // 恢复画布设置

```

### 注意事项/坑点

###### @state直接修饰Date类型，直接报错，解决方法：定义一个class，包含该Date

###### 在EntryAbility中声明PersistentStorage.PersistProp不生效

###### @Preview修饰的组件可以预览，但不能测交互，需在@Entry修饰的页面测交互