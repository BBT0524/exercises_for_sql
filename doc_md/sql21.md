# [sql21](https://www.nowcoder.com/practice/fc7344ece7294b9e98401826b94c6ea5?tpId=82&&tqId=29773&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

##1. 题目描述

查找所有员工自入职以来的薪水涨幅情况，给出员工编号emp_no以及其对应的薪水涨幅growth，并按照growth进行升序

（注:可能有employees表和salaries表里存在记录的员工，有对应的员工编号和涨薪记录，但是已经离职了，离职的员工salaries表的最新的to_date!='9999-01-01'，这样的数据不显示在查找结果里面）

CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL, -- '入职时间'
PRIMARY KEY (`emp_no`));
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL, -- '一条薪水记录开始时间'
`to_date` date NOT NULL, -- '一条薪水记录结束时间'
PRIMARY KEY (`emp_no`,`from_date`));

## 2 .解法：比较子查询

基本方法：
通过子查询从salaries中读取当前工资、入职时工资，然后作差。但是要注意的是，每个员工的入职时间存在于employees表中，因此查询入职工资的时候需要连接两表。此外，两个子查询返回的表仍需要通过emp_no对齐，这样作差的结果才是每个员工入职以来的薪资涨幅。


```sql
select s.emp_no, (s.salary - s2.salary) growth
from (
    -- 查找当前工资
    select emp_no, salary
    from salaries
    where to_date = '9999-01-01'
) s
join (
    -- 查找入职时的工资
    select e.emp_no, s1.salary
    from employees e
    join salaries s1
    on e.emp_no = s1.emp_no
    and e.hire_date = s1.from_date
) s2
on s.emp_no = s2.emp_no
order by growth
```



