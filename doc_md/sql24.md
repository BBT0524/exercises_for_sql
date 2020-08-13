# [sql24](https://www.nowcoder.com/practice/8fe212a6c71b42de9c15c56ce354bebe?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

获取所有非manager员工当前的薪水情况，给出dept_no、emp_no以及salary ，当前表示to_date='9999-01-01'

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
CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));
```

## 2.解法

必须得要说的东西，因为题目对于每个表的内容没有太多的介绍，下面说的是可能的情形：

1. 四个表之间的关系如下图：
   
    <center>
    <img src="https://img-blog.csdnimg.cn/20200813171451667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODI5MjU3MA==,size_16,color_FFFFFF,t_70#pic_center" width="70%">
    <div>
    图1 四表之间关系的可能情形
    </div>
    </center>

2. 也就是说employees中存在有的员工不属于任何部门，可能是因为还没有给员工分好部门。

### 2.1 内连接四表联查

非manager员工指的是：从employees, salaries, dept_emp 三表的emp_no部分的交集中减去dept_manager部分。


```sql
select de.dept_no, e.emp_no, s.salary 
from employees e 
join dept_emp  de on e.emp_no = de.emp_no 
join salaries  s on e.emp_no = s.emp_no 
and de.to_date='9999-01-01' 
and s.to_date='9999-01-01' 
and e.emp_no not in(
    select emp_no 
    from dept_manager 
    where to_date='9999-01-01' 
);
```

### 2.2 三表联查

仔细分析发现，其实三表联查就足够解题的了。因为题目隐含要求是返回有部门归属的非manager员工的“dept_no、emp_no、salary”信息。也就是说光有 salaries, dept_emp, dept_manager三个表就足以。

非manager员工指的是：从salaries, dept_emp 三表的emp_no部分的交集中减去dept_manager部分。

```sql
select de.dept_no, de.emp_no, s.salary
from  salaries s, dept_emp de
where de.emp_no = s.emp_no
and s.to_date = '9999-01-01'
and de.to_date = '9999-01-01'
and de.emp_no not in (
    select dm.emp_no
    from dept_manager dm
    where dm.to_date = '9999-01-01'
)
```

### 2.3 使用left join 查补集

使用left join 替代上面的 not in

```sql
select de.dept_no, de.emp_no, s.salary
from  salaries s
join dept_emp de on de.emp_no = s.emp_no
and s.to_date = '9999-01-01'
and de.to_date = '9999-01-01'
left join dept_manager dm on dm.emp_no = de.emp_no
and dm.to_date = '9999-01-01'
where dm.emp_no is NULL
```

### 2.4 从字段考虑

主要的想法就是逐步确定非manager员工的三个字段“dept_no、emp_no、salary”

1. 先确定非manager员工的 emp_no, dept_no 字段。通过dept_emp 和 dept_manager两表利用差集except完成。
2. 将1中查询的表和salaries内联，进而查询到非manager员工的工资salary字段。

```sql
select f.dept_no, f.emp_no, s.salary
from (
    select de.emp_no, de.dept_no
    from dept_emp de
    where de.to_date = '9999-01-01'
    except
    select dm.emp_no, dm.dept_no
    from dept_manager dm
    where dm.to_date = '9999-01-01'
) f -- 非manager员工的emp_no
join salaries s on s.emp_no = f.emp_no
where s.to_date = '9999-01-01'
```