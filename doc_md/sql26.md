# [sql26](https://www.nowcoder.com/practice/4bcb6a7d3e39423291d2f7bdbbff87f8?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

汇总各个部门当前员工的title类型的分配数目，即结果给出部门编号dept_no、dept_name、其部门下所有的当前(dept_emp.to_date = '9999-01-01')员工的当前(titles.to_date = '9999-01-01')title以及该类型title对应的数目count
(注：因为员工可能有离职，所有dept_emp里面to_date不为'9999-01-01'就已经离职了，不计入统计，而且员工可能有晋升，所以如果titles.to_date 不为 '9999-01-01'，那么这个可能是员工之前的职位信息，也不计入统计)

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
CREATE TABLE IF NOT EXISTS `titles` (
`emp_no` int(11) NOT NULL,
`title` varchar(50) NOT NULL,
`from_date` date NOT NULL,
`to_date` date DEFAULT NULL);
```

## 2. 解法: 内连接表+聚合

将三张表内连接在一起，形成只含有当前部门当前员工的title信息的表。然后对这个表按照三个字段de.dept_no, dp.dept_name, t.title进行聚合，并使用count(*)查询其中条数。

```sql
select de.dept_no, dp.dept_name, t.title, count(*)
from dept_emp de
join departments dp on de.dept_no = dp.dept_no
join titles t on t.emp_no = de.emp_no
and t.to_date = '9999-01-01'
and de.to_date = '9999-01-01'
group by de.dept_no, dp.dept_name, t.title
```

