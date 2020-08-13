# [sql25]()

## 1. 题目描述

获取员工其当前的薪水比其manager当前薪水还高的相关信息，当前表示to_date='9999-01-01',
结果第一列给出员工的emp_no，
第二列给出其manager的manager_no，
第三列给出该员工当前的薪水emp_salary,
第四列给该员工对应的manager当前的薪水manager_salary

```sql
CREATE TABLE `dept_emp` (
`emp_no` int(11) NOT NULL,
`dept_no` char(4) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`dept_no`));
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
```

## 2. 解法: 多表联查

给dept_emp, dept_manager两表各自连接一个salaries表：s1， s2。而题目的要求就是查找s1.salary > s2.salary 的记录项

```sql
select de.emp_no emp_no, dm.emp_no manager_no, s1.salary emp_salary, s2.salary  manager_salary
from dept_manager dm, dept_emp de, salaries s1, salaries s2
where dm.dept_no = de.dept_no
and s1.emp_no = de.emp_no
and s2.emp_no = dm.emp_no
and s1.to_date = '9999-01-01'
and s2.to_date = '9999-01-01'
and s1.salary > s2.salary
```