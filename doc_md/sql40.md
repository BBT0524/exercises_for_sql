# [sql40](https://www.nowcoder.com/practice/119f04716d284cb7a19fba65dd876b03?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

存在actor表，包含如下列信息：

```sql
CREATE TABLE IF NOT EXISTS actor (
actor_id smallint(5) NOT NULL PRIMARY KEY,
first_name varchar(45) NOT NULL,
last_name varchar(45) NOT NULL,
last_update timestamp NOT NULL DEFAULT (datetime('now','localtime')));
现在在last_update后面新增加一列名字为create_date, 类型为datetime, NOT NULL，默认值为'0000-00-00 00:00:00'
```

## 2. 解法

```sql
alter table actor add create_date datetime 
not NULL default '0000-00-00 00:00:00'
```

在某一列后面添加一个新列的语法：

```sql
-- 批量加列：在列column_x后面添加新列create_date，datetime类型，默认值'0000-00-00 00:00:00'
alter table actor add column_x
create_date datetime NOT NULL default('0000-00-00 00:00:00')
```

关于添加新行的练习题见sql34。