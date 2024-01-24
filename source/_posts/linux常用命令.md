---
title: linux常用命令
date: 2023-09-27 17:20:00
description: linux常用命令合集
tags:
- linux
categories:
- linux
---

<a href="/book/Linux命令大全.chm">Linux命令大全</a>

<h3>常用命令</h3>

```text
1.移动文件:my[源文件][目标文件]
2.删除文件或目录:rm -d 删除目录 -强制删除 -r递归删除[文件或目录名]
3.修改文件或目录群组: chgrp -r 递归[群组][文件或目录]
4.修改文件或目录权限: chmod r 递归[权限范围][文件或目录]
5.修改文件或目录拥有者:chown r 递归[权限围][文件或目录]
6.复制文件或目录: cp -f强制 -r 递归 [源文件][目标文件]
7.切换目录: cd [目的目录]
8.显示目录下文件: ls -a所有 -详细格式列表 递归
9. 创建目录:mkdir -p 如果上级目录没有建立则一并创建 [目录名称]
10.显示当前绝对路径:pwd
11.删除目录:rmdir.-p 删除后如果上级目录为空一并删除[目录名称]
12.变更用户身份: su [账户]
13.以其它身份执行命令: sudo -u [账户] [指令]
14.添加用户: adduser -g [群组] -e[权限] -u[账号]
15.删除用户: userdel -f 删除用户登录目录以及目录中的文件 [用户账户]
16. Ubuntu 初始化 root: udo passwd
17.重启: reboot
18.更改ip等网络设置: setup
19.服务更改生效命令: service network restart
20.关机: halt
```
