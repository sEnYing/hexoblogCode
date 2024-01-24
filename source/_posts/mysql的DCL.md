---
title: mysql的DCL
date: 2023-09-26 16:27:14
description: mysql的DCL相关指令
tags:
  - mysql
categories:
  - mysql
---

```text
● 创建用户：
  ○ 语法：
    ■ CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码'
● 用户授权：
  ○ 语法：
    ■ GRANT 权限1，权限2.。。。。ON 数据库名.表 TO '用户名'@'主机名'
● 查看用户权限：
  ○ 语法：
    ■ SHOW GRANTS FOR '用户名'@'主机名'
● 删除用户：
  ○ 语法：
    ■ DROP USER '用户名'@'主机名'
```
