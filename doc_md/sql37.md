# [sql37](https://www.nowcoder.com/practice/e1824daa0c49404aa602cf0cb34bdd75?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

针对如下表actor结构创建索引：
(注:在 SQLite 中,除了重命名表和在已有的表中添加列,ALTER TABLE 命令不支持其他操作)

```sql
CREATE TABLE IF NOT EXISTS actor (
actor_id smallint(5) NOT NULL PRIMARY KEY,
first_name varchar(45) NOT NULL,
last_name varchar(45) NOT NULL,
last_update timestamp NOT NULL DEFAULT (datetime('now','localtime')))
对first_name创建唯一索引uniq_idx_firstname，对last_name创建普通索引idx_lastname
(请先创建唯一索引，再创建普通索引)
```

## 2. 解法

```sql
-- 创建唯一索引
create unique index  uniq_idx_firstname 
on actor(first_name);
-- 创建普通索引
create index idx_lastname 
on actor(last_name);
```

注：sql39创建强制索引。