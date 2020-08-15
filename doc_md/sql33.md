# [sql33](https://www.nowcoder.com/practice/ac233de508ef4849b0eeb4f38dcf09cf?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

创建一个actor表，包含如下列信息(注：sqlite获取系统默认时间是datetime('now','localtime'))

列表	| 类型	| 是否为NULL	|含义
---|---|---|---
actor_id	|smallint(5)|	not null|	主键id
first_name	|varchar(45)|	not null|	名字
last_name	|varchar(45)|	not null|	姓氏
last_update	|timestamp|	not null|	最后更新时间，默认是系统的当前时间

## 2. 解法

```sql
create table if not exists actor (
    actor_id smallint(5) not null primary key,
    first_name varchar(45) not null,
    last_name varchar(45) not null,
    last_update timestamp not null default(datetime('now','localtime'))
)
```