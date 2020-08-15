# [sql39](https://www.nowcoder.com/practice/f9fa9dc1a1fc4130b08e26c22c7a1e5f?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

针对salaries表emp_no字段创建索引idx_emp_no，查询emp_no为10005, 使用强制索引。

```sql
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));
create index idx_emp_no on salaries(emp_no);
```

## 2. 解法

在sqlite中强制索引是 indexed by 待创建索引字段名

```sql
select * 
from salaries 
indexed by idx_emp_no 
where emp_no = 10005
```

## 3. 有关强制索引

- 在处理百万级数据查询的时候，一般查询会很慢。
- 如果建立联合索引，但是数据库存在多条索引的情况下，索引的执行依旧是全部执行，这种情况下依旧查询很慢。
- 所以要按照特定的索引执行，就必须使用强制索引才可能实现较快速查询。


## 参考
- https://developer.aliyun.com/article/624250