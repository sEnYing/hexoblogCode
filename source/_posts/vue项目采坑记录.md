---
title: vue项目采坑记录
date: 2023-09-25 14:59:04
description: 开发过程中遇到的坑记录，避免重复踩坑
tags: bug
categories: bug
---
```ts
// 1.tree全部展开折叠 
for (
    var i = 0;
    i < (this as any).$refs.tree.store._getAllNodes().length;
i++
) {
    (this as any).$refs.tree.store._getAllNodes()[
        i
        ].expanded = this.select.expandAll
}
// 2.el-tree回显默认选中项，set的各种方法不生效，需使用defaultCheckKeys
// 3.vue3引入图片需要用require，否则会报错
```
