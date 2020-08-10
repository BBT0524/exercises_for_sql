# 1.[sql19](https://www.nowcoder.com/practice/5a7975fabe1146329cee4f670c27ad55?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

## 题目描述

查找所有员工的last_name和first_name以及对应的dept_name，也包括暂时没有分配部门的员工
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
CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,

PRIMARY KEY (`emp_no`));

# 2. 解法

## 2.1 子查询+内连接

关键是键 emp_no 和 dept_name没有直接联系。

先将  表departments 和 dept_emp 通过 键dept_no连接，然后组成临时表并返回 de.emp_no, ds.dept_no, ds.dept_name， 这样便建立起了 键 emp_no 和 dept_name的联系。

```sql
select e.last_name, e.first_name, f.dept_name
from employees e
left join (
    select de.emp_no, ds.dept_no, ds.dept_name
    from departments ds
    join dept_emp de
    on de.dept_no = ds.dept_no    
) f
on e.emp_no = f.emp_no
```

## 2.2 两次左连接

关键是键 emp_no 和 dept_name没有直接联系。

通过两次左连接建立临时表，从而将 键 emp_no 和 dept_name建立联系。

- 对表按这个方式左连接 employees  -(1)-  dept_emo -(2)-  departments
- 其中，键（1）是 emp_no,  键（2）是dept_no

```sql
select e.last_name, e.first_name, ds.dept_name
from employees e
left join dept_emp  de 
on e.emp_no = de.emp_no
left join departments ds
on ds.dept_no = de.dept_no
```

