# 1.[sql 14](https://www.nowcoder.com/practice/c59b452f420c47f48d9c86d69efdff20?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

## 题目描述

从titles表获取按照title进行分组，每组个数大于等于2，给出title以及对应的数目t。
注意对于重复的emp_no进行忽略(即emp_no重复的title不计算，title对应的数目t不增加)。
CREATE TABLE IF NOT EXISTS `titles` (
`emp_no` int(11) NOT NULL,
`title` varchar(50) NOT NULL,
`from_date` date NOT NULL,

`to_date` date DEFAULT NULL);

# 2. 解法

分析：题目除了按照title分组和每组个数t要求外，还规定重复的emp_no 不重复计算到t中。

## 2.1 计数非重复的 emp_no 

因此要在分组中的emp_no去重后基础上计算t

```sql
select title, count(distinct emp_no) t
from titles
group by title
having t > 1
```



## 2.2 对去重后的子表进行分组

```sql
select t2.title, count(*) t
from (select distinct title, emp_no
      from titles) t2
group by t2.title
having t > 1
```



## 2.3 使用 group by 替换 2.2 中的 distinct

```sql
select t2.title, count(*) t
from (select title, emp_no
      from titles
      group by title, emp_no) t2
group by t2.title
having t > 1
```

