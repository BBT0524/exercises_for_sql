# [sql22](https://www.nowcoder.com/practice/6a62b6c0a7324350a6d9959fa7c21db3?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

统计各个部门的工资记录数，给出部门编码dept_no、部门名称dept_name以及部门在salaries表里面有多少条记录sum

```sql
CREATE TABLE `departments` (
`dept_no` char(4) NOT NULL,
`dept_name` varchar(40) NOT NULL,
PRIMARY KEY (`dept_no`));
CREATE TABLE `dept_emp` (
`emp_no` int(11) NOT NULL,
`dept_no` char(4) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`dept_no`));
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));
```

## 2. 解法 

要查询的字段dept_no，dept_name，salaries 表里面有多少条记录sum，零散分布在三个表中，所以需要联表查询。我理解的联表就是重新拼接新的表，我们要拼接的新表就是为 salaries表中每条记录匹配上字段dept_no，dept_name。一共有两种方式可以实现。

### 2.1 解法1：先两表联查，然后再聚合

```sql
select dp.dept_no, dp.dept_name, count(sd.emp_no) sum
from departments dp
join (
    select de.dept_no, s.emp_no
    from dept_emp de
    join salaries s
    on s.emp_no = de.emp_no
) sd
on sd.dept_no = dp.dept_no
group by dp.dept_no, dp.dept_name
```

### 2.2 解法2：三表内连接，然后聚合

```sql
select dp.dept_no, dp.dept_name, count(*) sum
from departments dp
join dept_emp de
on dp.dept_no = de.dept_no
join salaries s
on s.emp_no = de.emp_no
group by dp.dept_no, dp.dept_name

```
