# [sql27](https://www.nowcoder.com/practice/eb9b13e5257744db8265aa73de04fd44?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

给出每个员工每年薪水涨幅超过5000的员工编号emp_no、薪水变更开始日期from_date以及薪水涨幅值salary_growth，并按照salary_growth逆序排列。
提示：在sqlite中获取datetime时间对应的年份函数为strftime('%Y', to_date)
(数据保证每个员工的每条薪水记录to_date-from_date=1年，而且同一员工的下一条薪水记录from_data=上一条薪水记录的to_data)

```sql
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));
```

## 2. 解法: 自连接

题目理解为查询员工**存在某一年涨薪超过5000**的记录。重复利用表salaries，通过自连接实现，相隔一年的薪水记录在同一行，并且薪水涨幅大于5000.

### 2.1 解法1：利用每条薪水记录to_date-from_date=1年

```sql
select s2.emp_no, s2.from_date, (s2.salary - s1.salary) salary_growth
from salaries s1, salaries s2
where s1.emp_no = s2.emp_no
and s1.to_date = s2.from_date
and s2.salary - s1.salary > 5000
order by salary_growth desc
```

表1：salaries
emp_no | salary |from_date | to_date
---------|----------|---------|---------
10001 | 52117 | 1986-06-26 | 1987-06-26
10001 | 62102 | 1987-06-26 | 1988-06-25
10002 | 72527 | 1996-08-03 | 1997-08-03
10002 | 72527 | 1997-08-03 | 1998-08-03
10002 | 72527 | 1998-08-03 | 1999-08-03
10003 | 43616 | 1996-12-02 | 1997-12-02
10003 | 43466 | 1997-12-02 | 1998-12-02

按照下面代码中的自连接方式得到下表2：

```sql
select s2.emp_no, s2.from_date, (s2.salary - s1.salary) salary_growth
from salaries s1, salaries s2
where s1.emp_no = s2.emp_no
and s1.to_date = s2.from_date
```

表2：没有salary限制的自连接结果
s1.emp_no | s1.salary |s1.from_date | s1.to_date | s2.emp_no | s2.salary |s2.from_date | s2.to_date
---------|----------|---------|---------|---------|----------|---------|---------|
10001 | 52117 | 1986-06-26 | 1987-06-26 |10001 | 62102 | 1987-06-26 | 1988-06-25
10002 | 72527 | 1996-08-03 | 1997-08-03 |10002 | 72527 | 1997-08-03 | 1998-08-03
10002 | 72527 | 1997-08-03 | 1998-08-03 |10002 | 72527 | 1998-08-03 | 1999-08-03
10003 | 43616 | 1996-12-02 | 1997-12-02 |10003 | 43466 | 1997-12-02 | 1998-12-02

然后在上述自连接基础上再限制 s2.salary - s1.salary > 5000
于是结果为表3。

表3：有差额5000限制的自连接结果
s1.emp_no | s1.salary |s1.from_date | s1.to_date | s2.emp_no | s2.salary |s2.from_date | s2.to_date
---------|----------|---------|---------|---------|----------|---------|---------|
10001 | 52117 | 1986-06-26 | 1987-06-26 |10001 | 62102 | 1987-06-26 | 1988-06-25

### 2.2 解法2：利用strftime('%Y', to_date)函数限定时间

```sql
select s2.emp_no, s2.from_date, (s2.salary - s1.salary) salary_growth
from salaries s1, salaries s2
where s1.emp_no = s2.emp_no
and (strftime('%Y',s2.to_date) - strftime('%Y',s1.to_date) = 1 
     or strftime('%Y',s2.from_date) - strftime('%Y',s1.from_date) = 1)  
and s2.salary - s1.salary > 5000
order by salary_growth desc
```

### 2.3 题目的疑惑

题目的描述似乎还有另一种理解：查询员工**年年涨薪**超过5000的记录。也就是任何一年的涨薪都在5000以上。 感觉这方面似乎描述的不是很清楚。