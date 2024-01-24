---
title: css实用非常用属性
date: 2023-09-22 10:46:33
description: css实用非常用属性,修改文字显示省略号，光标颜色，裁剪以及可替换元素的内容宽高
tags: 
- css
categories: 
- css
---

```css
/* 1.可以把 块容器 中的内容限制为指定的行数。并且在超过行数后，在最后一行显示"..." */
display: -webkit-box; /*值必须为-webkit-box或者-webkit-inline-box*/
-webkit-box-orient: vertical; /*值必须为vertical*/
-webkit-line-clamp: 2; /*值为数字，表示一共显示几行*/
overflow: hidden;
/* 2.定义插入光标（caret）的颜色*/
caret-color: red;
/* 3.clip-path 属性使用裁剪方式创建元素的可显示区域，shape-outside 定义了一个可以是 非矩形的形状，相邻的内联内容应围绕该形状进行包裹*/
clip-path: none;
shape-outside: none
/* 4.object-fit 属性指定可替换元素的内容应该如何适应到其使用的高度和宽度确定的框。object-position 属性来指定被替换元素的内容对象在元素框内的对齐方式。*/
object-fit: fill;
object-position: 50px 50px;   /*距离左侧10px，距离顶部10%*/
/*5. max-content / min-content / fill-available / fit-content*/
/*这几个值都可用在 width, height, min-width, min-height, max-width 和 max-height 属性上。
display 必须为 inline-block 或者 block，否则上面的值不起作用。*/

```
