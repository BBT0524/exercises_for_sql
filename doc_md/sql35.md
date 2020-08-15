# [sql35](https://www.nowcoder.com/practice/153c8a8e7805400ba8e384e03acc6b3e?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

对于表actor批量插入如下数据,如果数据已经存在，请忽略(不支持使用replace操作)

```sql
CREATE TABLE IF NOT EXISTS actor (
actor_id smallint(5) NOT NULL PRIMARY KEY,
first_name varchar(45) NOT NULL,
last_name varchar(45) NOT NULL,
last_update timestamp NOT NULL DEFAULT (datetime('now','localtime')))
```

actor_id|	first_name|	last_name|	last_update
---|---|---|---
'3'|	'ED'|	'CHASE'|	'2006-02-15 12:34:33'

## 2. 解法

```sql
insert or ignore into actor values
('3', 'ED', 'CHASE','2006-02-15 12:34:33')
```