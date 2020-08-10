# 1.[sql20](https://www.nowcoder.com/practice/c727647886004942a89848e2b5130dc2?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

## 题目描述

查找员工编号emp_no为10001其自入职以来的薪水salary涨幅(总共涨了多少)growth(可能有多次涨薪，没有降薪)
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));

# 2. 解法

## 2.1 最大值减去最小值

因为没有降薪， 所以可以使用最大值减去最小值得到薪资涨幅

```sql
select max(salary) - min(salary) as growth
from salaries 
where emp_no = 10001
```



## 2.2 最后一次薪资 减去 入职薪资

如果有的时候薪资会有降薪的情况，那么方法2.1就不再适用。重新审题，查找员工编号emp_no为10001其自入职以来的薪水salary涨幅。那就是求最后一次薪资减去第一次薪资。

注意：

- 第一次和最后一次的薪资都需要两层子查询， 分别确定的是时间和薪资。

```sql
select (
    select salary
    from salaries
    where emp_no = 10001
    and to_date = ( 
        select max(to_date)
        from salaries
        where emp_no = 10001
        )
) - (
    select salary
    from salaries
    where emp_no = 10001
    and to_date = ( 
        select min(to_date)
        from salaries
        where emp_no = 10001
        )
) growth
```

