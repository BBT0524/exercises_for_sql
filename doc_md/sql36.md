# [sql36](https://www.nowcoder.com/practice/881385f388cf4fe98b2ed9f8897846df?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

对于如下表actor，其对应的数据为:

actor_id|	first_name|	last_name|	last_update
---|---|---|---
1|	PENELOPE|	GUINESS|	2006-02-15 12:34:33
2|	NICK|	WAHLBERG|	2006-02-15 12:34:33

请你创建一个actor_name表，并且将actor表中的所有first_name以及last_name导入该表.
actor_name表结构如下：

列表|	类型|	是否为NULL|	含义
---|---|---|---
first_name|	varchar(45)|	not null|	名字
last_name|	varchar(45)|	not null|	姓氏

## 2. 解法

```sql
create table actor_name as 
select first_name,last_name from actor;
```