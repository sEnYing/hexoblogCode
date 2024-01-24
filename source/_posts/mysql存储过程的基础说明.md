---
title: mysql存储过程的基础说明
date: 2023-09-26 16:25:43
description: mysql的存储过程相关的基础使用方法
tags:
- mysql
categories:
- mysql
---

```text
● SQL语句的合并，中间加入逻辑控制
● 优缺点：
  ○ 优点：
    ■ 调试完成就可以稳定运行（业务需求相对稳定的情况）
    ■ 存储过程可以减少业务系统与数据库的交互
  ○ 缺点：
    ■ 业务需求变化快即较少使用存储过程
    ■ 移植十分困难
  ○ 语法格式：
    ■ DELIMITER $$
CREATE PROCEDURE 	存储过程名称（）
BEGIN
.
.
.
END $$
    ■ 调用：CALL 存储过程名称
  ○ 语法2：传参
    ■ DELIMITER $$
CREATE PROCEDURE 存储过程名（IN 参数名 参数类型	）
。
。
。
END $$
    ■ 调用：CALL 存储过程名称（xxx）
  ○ 语法3：获取返回值
    ■ DELIMITER $$
CREATE PROCEDURE 存储过程名（IN 参数名 参数类型	）
。
。
SET @out-name = 1；

SELECT @out-name
END $$
    ■ 调用：CALL 存储过程名称（xxx）
```
