# [sql31](https://www.nowcoder.com/practice/18f30bb19fd34abebcf7e6397c7fb5d8?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

获取select * from employees对应的执行计划

用例输入：
INSERT INTO employees VALUES(10001,'1953-09-02','Georgi','Facello','M','1986-06-26');
INSERT INTO employees VALUES(10002,'1964-06-02','Bezalel','Simmel','F','1985-11-21');
INSERT INTO employees VALUES(10003,'1959-12-03','Parto','Bamford','M','1986-08-28');
INSERT INTO employees VALUES(10004,'1954-05-01','Chirstian','Koblick','M','1986-12-01');
INSERT INTO employees VALUES(10005,'1955-01-21','Kyoichi','Maliniak','M','1989-09-12');
INSERT INTO employees VALUES(10006,'1953-04-20','Anneke','Preusig','F','1989-06-02');
INSERT INTO employees VALUES(10007,'1957-05-23','Tzvetan','Zielinski','F','1989-02-10');
INSERT INTO employees VALUES(10008,'1958-02-19','Saniya','Kalloufi','M','1994-09-15');
INSERT INTO employees VALUES(10009,'1952-04-19','Sumant','Peac','F','1985-02-18');
INSERT INTO employees VALUES(10010,'1963-06-01','Duangkaew','Piveteau','F','1989-08-24');
INSERT INTO employees VALUES(10011,'1953-11-07','Mary','Sluis','F','1990-01-22');

用例输出：
0|Init|0|12|0||00|None
1|OpenRead|0|2|0|6|00|None
2|Rewind|0|11|0||00|None
3|Column|0|0|1||00|None
4|Column|0|1|2||00|None
5|Column|0|2|3||00|None
6|Column|0|3|4||00|None
7|Column|0|4|5||00|None
8|Column|0|5|6||00|None
9|ResultRow|1|6|0||00|None
10|Next|0|3|0||01|None
11|Halt|0|0|0||00|None
12|Transaction|0|0|1|0|01|None
13|Goto|0|1|0||00|None

## 2. 解法

explain模拟优化器执行SQL语句，在5.6以及以后的版本中，除过select，其他比如insert，update和delete均可以使用explain查看执行计划，从而知道mysql是如何处理sql语句，分析查询语句或者表结构的性能瓶颈。
作用:
1、表的读取顺序
2、数据读取操作的操作类型
3、哪些索引可以使用
4、哪些索引被实际使用
5、表之间的引用
6、每张表有多少行被优化器查询

具体解释参见[这个链接](https://www.runoob.com/sqlite/sqlite-explain.html)。

```sql
explain
select * 
from employees
```