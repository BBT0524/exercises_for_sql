# 1.[sql12 所有部门中当前员工的当前薪水最高的相关信息](https://www.nowcoder.com/practice/4a052e3e1df5435880d4353eb18a91c6?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

获取所有部门中当前(dept_emp.to_date = '9999-01-01')员工当前(salaries.to_date='9999-01-01')薪水最高的相关信息，给出dept_no, emp_no以及其对应的salary。
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

# 2. 解法

## 2.1 正确的做法： 子查询

关联子查询，父表固定一个部门，子表进行子查询

```sql
select d.dept_no, s.emp_no, s.salary
from salaries s
join dept_emp d
on s.emp_no = d.emp_no
and s.to_date = '9999-01-01'
and d.to_date = '9999-01-01'
where s.salary in ( select max(s2.salary)
                   from dept_emp d2
                   join salaries s2
                   on s2.emp_no = d2.emp_no
                   and d2.to_date = '9999-01-01'
                   and s2.to_date = '9999-01-01'
                   and d2.dept_no = d.dept_no)  -- 子表和父表结果关联 
order by d.dept_no
```

或者这样：

```sql
select d.dept_no, s.emp_no, s.salary
from salaries s
join dept_emp d
on s.emp_no = d.emp_no
and s.to_date = '9999-01-01'
and d.to_date = '9999-01-01'
where s.salary in ( select max(s2.salary)
                   from dept_emp d2
                   join salaries s2
                   on s2.emp_no = d2.emp_no
                   and d2.to_date = '9999-01-01'
                   and s2.to_date = '9999-01-01'
                   where d2.dept_no = d.dept_no) -- 子表和父表结果关联
order by d.dept_no
```



## 2.1  group by的错误使用例子

虽然这部分代码可以通过，但是违反了下面group by的第一个知识点，所以通过的代码是有问题的：

- 使用group by子句时，select子句中只能含有聚合的键（group by 后面的列名）、聚合函数、常数。
- **GROUP BY 默认取非聚合的第一条记录**



分析：题目的要求分组筛选，即返回每个部门的当前员工里，当前薪资最高的人。也就是说，当前状态下，每个部门的最高薪资员工。

思路：按照部门聚合，然后返回每个部门符合要求员工的最高薪资者。

- 先通过子查询返回符合要求的员工集合。（要求是：当前员工，当期部门，薪资降序）
- 在符合要求的员工集合中，按部门聚合，并返回每个部门薪资最高的人/人们。

```sql
select f.dept_no, f.emp_no, max(f.salary)
from  ( select d.dept_no, s.emp_no, s.salary
        from salaries s, dept_emp d
        where s.emp_no = d.emp_no
              and s.to_date = '9999-01-01'
              and d.to_date = '9999-01-01'
        order by s.salary desc ) as f
group by f.dept_no
```

