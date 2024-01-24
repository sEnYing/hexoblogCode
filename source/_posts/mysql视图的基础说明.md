---
title: mysql视图的基础说明
date: 2023-09-26 16:23:57
description: mysql的视图相关的基础使用方法
tags:
  - mysql
categories:
  - mysql
---

```text
● 由查询结果形成的一张虚拟的表
● 作用：
  ○ 权限控制
  ○ 复杂的多表查询
● 某个查询结果出现频繁，并且查询语句复杂，就可以根据查询语句创建视图方便查询
● 语法：
  ○ CREATE VIEW 视图名[字段列表] AS SELECT 查询语句
● 查询：
  ○ SELECT XXX FROM 视图名 WHERE XXXX查询条件
● 视图与表的区别：
  ○ 建立在表的基础上
  ○ 通过视图，不要进行增删改操作，视图主要用来简化查询
  ○ 删除视图，表不受影响，删除表，视图就不在起作用。
```
