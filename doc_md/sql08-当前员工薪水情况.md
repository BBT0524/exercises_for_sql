# 1. [sql08题目描述](https://www.nowcoder.com/practice/ae51e6d057c94f6d891735a48d1c2397?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

找出所有员工当前(to_date='9999-01-01')具体的薪水salary情况，对于相同的薪水只显示一次,并按照逆序显示
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));

# 2. 解法

## 2.1 distinct 限定返回值不重复

仅限在数据量很小的时候使用distinct

```sql
select distinct salary
from salaries
where to_date = '9999-01-01'
order by salary desc
```

## 2.2 group by

当数据量比较大，以及系统对高并发性能要求很高时，一般用distinct效率不高，常使用group by 实现去重的功能。

```sql
select  salary
from salaries
where to_date = '9999-01-01'
group by salary
order by salary desc
```



