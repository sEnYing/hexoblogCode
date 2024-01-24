---
title: mysql数据库的备份和还原
date: 2023-09-26 16:28:00
description: mysql数据库的备份和还原指令
tags:
  - mysql
categories:
  - mysql
---

```text
● 备份语法：
  ○ mysqldump -u用户名 -p密码 数据库名 > 文件路径
● 还原语法：
  ○ source sql文件地址
● mysql8修改密码语法：
  ○ ALTER USER 'root'@'localhost' IDENTIFIED BY '你的新密码';
● mysql8之前语法：
  ○ set password for 'root'@'localhost'=password('新密码');	
```
