# 1.[sql06题目描述](https://www.nowcoder.com/practice/23142e7a23e4480781a3b978b5e0f33a?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=%2Fta%2Fsql&qru=%2Fta%2Fsql%2Fquestion-ranking)

查找所有员工入职时候的薪水情况，给出emp_no以及salary， 并按照emp_no进行逆序(请注意，一个员工可能有多次涨薪的情况)
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

# 2. 解法

关键是如何确定员工入职时的记录项。

## 2.1 方法1：hire_data 和from_data一致的项是员工入职项

```sql
select e.emp_no, s.salary
from employees e
join salaries s
on e.emp_no = s.emp_no
and e.hire_date = s.from_date
order by e.emp_no desc
```

## 2.2 方法2：纯where实现方法1

```sql
select s.emp_no, s.salary
from employees e, salaries s
where e.emp_no = s.emp_no
      and e.hire_date = s.from_date
order by s.emp_no desc
```



## 2.3方法3：salaries中from_data中最小的项是员工入职项

```sql
select emp_no, salary
from salaries
group by emp_no         -- 按照emp_no聚合成不同集合
having min(from_date)   -- 返回集合中最小的from_date项
order by emp_no desc
```

 having 用来对 group by 进行筛选, 可以使用一些聚合函数 count, max, avg, sum.
