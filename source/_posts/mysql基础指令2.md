---
title: mysql基础指令2
date: 2023-09-26 16:18:55
description: mysql的基础指令，指令使用不太熟练，省的总去百度
tags:
- mysql
categories:
- mysql
---

```text
1. 单列排序：SELECT XXXX（字段名） FROM XXXX（表名） [WHERE XXXX（字段名） = XXXX（值）] ORDER BY XXXX（字段名） [ASC/DESC]      DESC降序排序
2. 组合排序：SELECT XXXX（字段名） FROM XXXX（表名） [WHERE XXXX（字段名） = XXXX（值）]  ORDER BY XXXX（字段名） [ASC/DESC],XXXX（字段名）[ASC/DESC]
  ○ 特点：如果第一个字段值相同，就按照第二个字段进行排序
3. 聚合函数：将一列数据作为一个整体，进行纵向计算，常用的聚合函数：
  ○ COUNT（字段）统计记录数
  ○ SUM（字段）求和操作
  ○ MAX（字段） 求最大值
  ○ MIN（字段） 求最小值
  ○ AVG（字段） 求平均值
● SELECT XXX（聚合函数） FROM XXXX（表名） [WHERE 条件语句]
  ○ SELECT COUNT（*） FROM XXXXX
  ○ SELECT COUNT（id） FROM XXXXX	
    ■ COUNT 在统计时会忽略空值，不要使用带空值的列使用COUNT
  ○ SELECT SUM（money）AS '总工资',MAX（money）'最高工资'  FROM XXXXX
  ○ SELECT COUNT（*）FROM XXXX WHERE XXX（字段名）>XXXX（字段值）
4.  分组查询：SELECT 分组字段/聚合函数 FROM XXXX [WHERE 条件语句] GROUP BY XXXX（分组字段）
  ○ SELECT XXX,AVG（XXX） FROM  XXX GROUP BY XXX
  ○ SELECT XXXX,SUM（XXX） FROM XXX WHERE XXX IS NOT NULL GROUP BY XXXX HAVING SUM（XXX）> XXX    HAVING后边为判断条件	
    ■ 分组之后，进行条件过滤使用：HAVING	判断条件
    ■ WHERE与HAVING的区别
      ● WHERE：
        a. 在分组前过滤
        b. 后面不能跟聚合函数
      ● HAVING：
        a. 在分组后进行过滤
        b. 后面可以写聚合函数
5. LIMIT:指定查询的数据条数
  ○ SELECT XXX FROM XXX  LIMIT offset，length   
    ■ offset：起始行数，默认从0开始
    ■ length：返回的行数
  ○ SELECT XXX FROM XXX LIMIT 0,10	  SELECT XXX FROM XXX LIMIT 10
6. 约束：指对数据进行一定的限制，来保证数据的完整性有效性正确性
  ○ 主键约束：PRIMARY KEY
    ■ 特点：不可重复，唯一，非空
    ■ 作用：用来表示数据库中的每一条记录
    ■ 语法格式：字段名   字段类型  PRIMARY KEY
    ■ 创建方式：
      1. CREATE TABLE emp (
id INT PRIMARY KEY,
name VARCHAR(20),
sex CHAR(1)
);
      1. CREATE TABLE emp (
id INT,
name VARCHAR(20),
sex CHAR(1),
PRIMARY KEY(id)   指定id为主键
)；
      2. ALTER TABLE emp ADD PRIMARY KEY(id)
    ■ 删除主键：
      ● ALTER TABLE emp DROP PRIMARY KEY
    ■ 主键自增：AUTO_INCREMENT   字段类型必须是整数类型
      ● DELETE和TRUNCATE对自增长的影响：
        ○ DELETE删除表中所有数据，将表中的数据逐条删除：对自增没有影响，主键继续往上增
        ○ TRUNCATE删除表中的所有数据，是将整个表删除，然后在创建一个结构相同的表：自增从1开始

  ○ 唯一约束：UNIQUE
    ■ 特点：某一列不能够重复（对null值不做唯一判断）
    ■ 语法格式：字段名 字段类型 UNIQUE
    ■ 和主键约束的区别：
      ● 主键约束是唯一不能为空的，唯一约束是唯一可以为空的
      ● 一个表中只能有一个主键，但是可以有多个唯一约束
  ○ 非空约束：NOT NULL
    ■ 特点：某一列不允许为空
    ■ 语法格式：字段名 字段类型 NOT NULL
  ○ 外键约束：FOREIGN KEY  
    ■ 作用：可以让两张表之间产生一个对应的关系，从而保证了主从表引用的完整性
    ■ 外键：从表中与主表的主键对应的字段
    ■ 主表和从表：
      ● 主表：主键id所在的表，一的一方
      ● 从表：外键字段所在的表，多的一方
    ■ 语法格式：
      ● 创建表的时候添加外键  CREATE TABLE 表名 (字段。。。。。。,[CONSTRAINT][外键约束名] FOREIGN KEY(外键字段名) REFERENCES 主表（主键字段）
    ■ 添加外键约束后，就会产生一个强制的外键约束检查，保证数据的完整性和一致性
    ■ 创建表之后添加外键：
      ● ALTER TABLE 表名 ADD FOREIGN KEY（外键字段名）REFERENCES 主表（主键字段）
    ■ 注意事项：
      ● 从表的外键类型必须与主表的主键类型一致
      ● 添加数据时，应该先添加主表的数据
      ● 删除数据的时候，要先删除从表中的数据
7. 默认值：用来指定某一列的默认值
  ○ 字段名 字段类型 default 默认值
8. 级联删除：
  ○ 删除主表的数据的同时，可以删除与之相关的从表中的数据
  ○ 语法格式：ON DELETE CASCADE
```
