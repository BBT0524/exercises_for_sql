# 1. [sql15](https://www.nowcoder.com/practice/a32669eb1d1740e785f105fa22741d5c?tpId=82&&tqId=29767&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

## 题目描述

查找employees表所有emp_no为奇数，且last_name不为Mary(注意大小写)的员工信息，并按照hire_date逆序排列(题目不能使用mod函数)
CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,

PRIMARY KEY (`emp_no`));

# 2. 解法

全数字的奇偶查询：

- 方法1：位运算。按位与 &;  奇数：a & 1 = 1；偶数：a & 1 = 0；
- 方法2： 求余。a % 2 = 1； a % 2 = 0;

## 2.1 位运算

```sql
select *
from employees 
where emp_no & 1 = 1  -- 也可以写为 where emp_no & 1 
and last_name <> 'Mary'
order by hire_date desc
```



## 2.2 求余

```sql
select *
from employees 
where emp_no % 2 = 1
and last_name <> 'Mary'
order by hire_date desc
```



#  参考

- https://blog.csdn.net/ccStroy/article/details/78061861