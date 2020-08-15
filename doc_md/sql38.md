# [sql38](https://www.nowcoder.com/practice/b9db784b5e3d488cbd30bd78fdb2a862?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

针对actor表创建视图actor_name_view，只包含first_name以及last_name两列，并对这两列重新命名，first_name为first_name_v，last_name修改为last_name_v：

```sql
CREATE TABLE IF NOT EXISTS actor (
actor_id smallint(5) NOT NULL PRIMARY KEY,
first_name varchar(45) NOT NULL,
last_name varchar(45) NOT NULL,
last_update timestamp NOT NULL DEFAULT (datetime('now','localtime')))
```

## 2. 解法

### 2.1  create view view_name as (要view的内容)

```sql
create view actor_name_view as
select first_name first_name_v, last_name last_name_v
from actor 
```

### 2.2  create view view_name(需要view的列名) as (要view的内容)

```sql
create view actor_name_view (first_name_v, last_name_v) as
select first_name, last_name from actor 
```