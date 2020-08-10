# 1.[sql17](https://www.nowcoder.com/practice/8d2c290cc4e24403b98ca82ce45d04db?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

## 题目描述

获取当前（to_date='9999-01-01'）薪水第二多的员工的emp_no以及其对应的薪水salary
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));

# 2. 解法

order by  + limit 1, 1

```sql
select emp_no, salary
from salaries 
where to_date = '9999-01-01'
order by salary desc
limit 1, 1   -- 从第1个开始，往后取一个。 或者 limit 1 offset 1， 抛弃1个后，取一个 
```

