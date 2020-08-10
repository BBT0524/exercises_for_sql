# 1.[sql16](https://www.nowcoder.com/practice/c8652e9e5a354b879e2a244200f1eaae?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

统计出当前(titles.to_date='9999-01-01')各个title类型对应的员工当前(salaries.to_date='9999-01-01')薪水对应的平均工资。结果给出title以及平均工资avg。
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));
CREATE TABLE IF NOT EXISTS "titles" (
`emp_no` int(11) NOT NULL,
`title` varchar(50) NOT NULL,
`from_date` date NOT NULL,

`to_date` date DEFAULT NULL);



# 2. 解法

聚合+avg函数

```sql
select t.title, avg(s.salary)
from salaries s
join titles t
on s.emp_no = t.emp_no
and s.to_date = '9999-01-01'
and t.to_date = '9999-01-01'
group by t.title
```

