# [sql30]()

## 1. 题目描述


film表
字段|	说明
---|---
film_id	|电影id
title	|电影名称
description|	电影描述信息

```sql
CREATE TABLE IF NOT EXISTS film (
film_id smallint(5)  NOT NULL DEFAULT '0',
title varchar(255) NOT NULL,
description text,
PRIMARY KEY (film_id));
```

category表

字段 | 说明
---|---
category_id	|电影分类id
name	|电影分类名称
last_update|电影分类最后更新时间

```sql
CREATE TABLE category  (
category_id  tinyint(3)  NOT NULL ,
name  varchar(25) NOT NULL, `last_update` timestamp,
PRIMARY KEY ( category_id ));
```

film_category表
字段|说明
---|---
film_id|电影id
category_id|电影分类id
last_update	|电影id和分类id对应关系的最后更新时间

```sql
CREATE TABLE film_category  (
film_id  smallint(5)  NOT NULL,
category_id  tinyint(3)  NOT NULL, `last_update` timestamp);
```

你能使用子查询的方式找出属于Action分类的所有电影对应的title,description吗

## 2. 解法

使用子查询查询类别为Action的 category_id, 由此限定要查询的电影。自然在此之前需要通过关联连接或者内连接将表film扩充电影类别属性字段category_id。

### 2.1 where版

```sql
select f.title, f.description
from film f, film_category fc
where f.film_id = fc.film_id
and fc.category_id = (select category_id
                      from category 
                      where name = 'Action' 
                     )
```

### 2.2 join 版

```sql
select f.title, f.description
from film f
join film_category fc
on f.film_id = fc.film_id
and fc.category_id = (select category_id
                      from category 
                      where name = 'Action' 
                     )
```