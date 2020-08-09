# 1.[sql03题目描述](https://www.nowcoder.com/practice/c63c5b54d86e4c6d880e4834bfd70c3b?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=%2Fta%2Fsql&qru=%2Fta%2Fsql%2Fquestion-ranking)

查找各个部门当前(dept_manager.to_date='9999-01-01')领导当前(salaries.to_date='9999-01-01')薪水详情以及其对应部门编号dept_no

(注:请以salaries表为主表进行查询，输出结果以salaries.emp_no升序排序，并且请注意输出结果里面dept_no列是最后一列)

CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL, -- '员工编号',
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));

CREATE TABLE `dept_manager` (
`dept_no` char(4) NOT NULL, -- '部门编号'
`emp_no` int(11) NOT NULL, -- '员工编号'
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`dept_no`));

# 2. 解法

## 2.1 解法1内连接：使用where连接两个表

```sql
select s.emp_no, s.salary, s.from_date, s.to_date, d.dept_no
from salaries s, dept_manager d
where d.emp_no = s.emp_no      -- 内连接的条件
      and d.to_date = '9999-01-01'
      and s.to_date = '9999-01-01'
order by s.emp_no
```

注意：因为存在换部门，甚至离职的情况，所以d.to_date='9999-01-01’是必须的

## 2.2 解法2内连接：使用inner join

这个和2.1的相等。即只连接等值部分记录。

```sql
select s.emp_no, s.salary, s.from_date, s.to_date,d.dept_no
from salaries s          -- 主表
inner join dept_manager d  
on d.emp_no = s.emp_no   -- 内连接的条件
where d.to_date = '9999-01-01'
      and s.to_date = '9999-01-01'
order by s.emp_no
```

其中 inner可以省略，又因为是以salaries为主表， 且主表中的emp.no是升序排列，所以 order by s.emp_no 可以省略

```sql
select s.emp_no, s.salary, s.from_date, s.to_date,d.dept_no
from salaries s            -- 主表
inner join dept_manager d
on d.emp_no = s.emp_no     -- 内连接的条件
where d.to_date = '9999-01-01'
      and s.to_date = '9999-01-01'
```



