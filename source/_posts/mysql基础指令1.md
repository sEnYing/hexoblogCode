---
title: mysql基础指令1
date: 2023-09-26 16:17:25
description: mysql的基础指令，指令使用不太熟练，省的总去百度
tags:
- mysql
categories:
- mysql
---

```text
1. 创建表：CREATE TABLE xxxx (参数 参数类型，参数 参数类型)；
2. 查看表结构：DESC XXXX
3. 查看所有表：SHOW TABLES
4. 重命名表：RENAME TABLE XXXX TO XXXXX  前旧后新
5. 修改表字符集：ALTER TABLE xxxx CHARACTER SET gbk 
6. 表中添加一个字段：ALTER TABLE xxx ADD xxxx(字段名) xxxx(字段类型)
7. 修改表中列的字段类型或字段长度：ALTER TABLE XXXX(表名) MODIFY xxxx(字段名) xxxx（字段类型）
8. 修改表列名：ALTER TABLE xxxx(表名) CHANGE xxxx（旧列名） xxxx（新列名） xxxx（字段类型）
9. 删除列：ALTER TABLE xxxx(表名) DROP xxxx（列名）
10. 插入数据：
  ○ 插入全部字段，全部字段名都写：INSERT INTO xxx（表名） (字段名1，字段名2) VALUES （字段值1，字段值2）
  ○ 插入全部字段，不写字段名：INSERT INTO xxx（表名）  VALUES （字段值1，字段值2）
  ○ 插入指定字段：INSERT INTO xxx（表名） (字段名1) VALUES （字段值1）
● 注意事项：
  ○ 值与字段必须对应，个数&数据类型&长度，都必须一致
  ○ 在插入 varchar char date类型时必须用引号包裹
  ○ 如果插入空值，可以忽略不写或者写null
11. 修改数据：
  ○ 修改列全部值：UPDATE XXXX（表名） SET xxxx（列名）= xxxx（值）
  ○ 修改选中字段：UPDATE XXXX（表名） SET xxxx（列名）= xxxx（值） WHERE  xxxx（字段名） =xxx（值） 
  ○ 一次性修改多个列：UPDATE XXXX（表名） SET xxxx（列名）= xxxx（值） ， xxxx（列名）= xxxx（值） WHERE  xxxx（字段名） =xxx（值） 
12. 删除数据：
  ○ 删除选中数据：DELETE FROM XXXX（表名） WHERE  XXXX（键名）=xxxx（键值）
  ○ 删除全部数据：DELETE FROM XXXX（表名） 不推荐，对表中数据逐条删除
  ○ 删除全部数据：TRUNCATE TABLE XXXXX（表名） 删除整张表，然后在创建一个一模一样的新表
13. 简单查询：
  ○ 查询表中所有数据：SELECT * FROM XXXX（表名）   *代表所有列
  ○ 查询所有数据，只显示id和name：SELECT  id，name FROM XXXX（表名） 
  ○ 查询所有数据，然后列名改成中文：SELECT  id AS “编号”， name AS “姓名”  FROM xxxx（表名）
  ○ 去重操作：SELECT DISTINCT XXXX（列名）FROM xxxx（表名）
  ○ 查到直接修改某列：SELECT xxxx（列名），xxxx（列名）+ 1000（要做什么操作） AS  xxxx（改回修改前的列名） FROM XXXX（表名）
● 注意：查询操作不会对数据库中的数据进行修改，仅是一种显示的方式
14. 条件查询：
  ○ SELECT XXXX（列名） FROM XXXX（表名） WHERE XXXX（键名）=xxxx（键值）（条件表达式）
  ○ 运算符：
    ■ 比较运算符：
      ● < > >= <= = !=
      ● BETWEEN......AND.......：显示某一区间的值  BETWEEN 2000 AND 10000
      ● IN（集合）：集合表示多个值，使用逗号分隔，例如：NAME IN （悟空,八戒）  IN中每个数据都会作为一次条件，只要满足就会显示
      ● LIKE '%张%'：模糊查询
      ● IS NULL：查询某一列为null的值，不能写 =NULL
    ■ 逻辑运算符：
      ● AND &&  多个条件同时成立
      ● OR  ||	多个条件任一成立
      ● NOT		不成立，取反
  ○ 查询举例：
    ■ SELECT * FROM XXXX WHERE  XXXX= XXXX
    ■ SELECT * FROM XXXX WHERE XXXX>XXXX
    ■ SELECT * FROM XXXX WHERE XXXX !=XXXX
    ■ SELECT * FROM XXXX WHERE XXXX  BETWEEN XXXX AND XXXX
    ■ SELECT * FROM XXXX WHERE XXXX  >= xxxxx AND XXXX<=xxxx
    ■ SELECT * FROM XXXX WHERE XXXX=XXXX OR XXXX=XXXX OR XXXX=XXXX
    ■ SELECT * FROM XXXX WHERE XXXX IN(XXXX,XXX,XXXX)
    ■ SELECT * FROM XXXX WHERE XXXX LIKE '%xxxx%'     %通配符 表示匹配任意多个字符串
    ■ SELECT * FROM XXXX WHERE XXXX LIKE '%xxxx'     以xxxx结尾
    ■ SELECT * FROM XXXX WHERE XXXX LIKE '_x'     _通配符，仅代表一个字符   匹配为XX


```
