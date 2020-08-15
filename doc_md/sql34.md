# [sql34](https://www.nowcoder.com/practice/51c12cea6a97468da149c04b7ecf362e?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

对于表actor批量插入如下数据(不能有2条insert语句哦!)

```sql
CREATE TABLE IF NOT EXISTS actor (
actor_id smallint(5) NOT NULL PRIMARY KEY,
first_name varchar(45) NOT NULL,
last_name varchar(45) NOT NULL,
last_update timestamp NOT NULL DEFAULT (datetime('now','localtime')))
```

actor_id|	first_name|	last_name|	last_update|
---|---|---|---
1|	PENELOPE|	GUINESS|	2006-02-15 12:34:33
2|	NICK|	WAHLBERG|	2006-02-15 12:34:33


## 2. 解法

```sql
insert into actor values 
(1,'PENELOPE', 'GUINESS','2006-02-15 12:34:33'),
(2,'NICK', 'WAHLBERG','2006-02-15 12:34:33')
```


## 3. 批量加行/列方法

```sql
加行： insert into 表名(列名) values (行内容),(行内容);
加列：alter table 表名 add column 列名 列约束，列名 列约束；
-- 批量加行
insert into actor values
(1,'PENELOPE','GUINESS','2006-02-15 12:34:33'),
(2,'NICK','WAHLBERG','2006-02-15 12:34:33');
-- 批量加列：在列column_x后面添加新列create_date，datetime类型，默认值'0000-00-00 00:00:00'
alter table actor add column_x
create_date datetime NOT NULL default('0000-00-00 00:00:00')
;
```

关于添加新列的练习题见sql40