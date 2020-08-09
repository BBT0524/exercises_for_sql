# 1.[sql09 所有部门当前manager的当前薪水情况](https://www.nowcoder.com/practice/4c8b4a10ca5b44189e411107e1d8bec1?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=%2Fta%2Fsql&qru=%2Fta%2Fsql%2Fquestion-ranking)

## 题目描述

获取所有部门当前(dept_manager.to_date='9999-01-01')manager的当前(salaries.to_date='9999-01-01')薪水情况，给出dept_no, emp_no以及salary(请注意，同一个人可能有多条薪水情况记录)
CREATE TABLE `dept_manager` (
`dept_no` char(4) NOT NULL,
`emp_no` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`dept_no`));
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));

# 2. 解法

## 2.1 join + where

内连接两张表，然后通过where筛选当前时间的记录项

```sql
select d.dept_no, d.emp_no, s.salary
from dept_manager d
join salaries s
on d.emp_no = s.emp_no
where d.to_date = '9999-01-01'
      and s.to_date = '9999-01-01'
```

## 2.2  纯where

```sql
select d.dept_no, d.emp_no, s.salary
from dept_manager d, salaries s
where d.emp_no = s.emp_no
      and d.to_date = '9999-01-01'
      and s.to_date = '9999-01-01'
```



 

