# [sql23](https://www.nowcoder.com/practice/b9068bfe5df74276bd015b9729eec4bf?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1)

## 1. 题目描述

对所有员工的当前(to_date='9999-01-01')薪水按照salary进行按照1-N的排名，相同salary并列且按照emp_no升序排列

```sql
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));
```


## 2. 解法

### 2.1 自连接 + 聚合

同sql18的解法2.2中提到的确定员工薪资排名的方法：按照 s1.salary <= s2.salary 方式自连接，然后按照字段 s1.emp_no, s1.salary 聚合分组，
那么 去重 s2.salary 后组内记录的条数就是题目要求的“相同salary并列排名”情况

```sql
select s1.emp_no, s1.salary, count(distinct s2.salary) rank
from salaries s1
join salaries s2
on s1.salary <= s2.salary
and s1.to_date = '9999-01-01'
and s2.to_date = '9999-01-01'
group by s1.emp_no, s1.salary   
order by rank, s1.emp_no
```

### 2.2 关联子查询

方法与 sql12相同，在父表固定一个 s1.emp_no, 然后再子表中查询它的rank值。而rank值的确定依旧用的是s1.salary <= s2.salary 方式自连接。
与sql12不同的是，这里rank的值是在**select语句中**进行关联子查询获得。而sql12是通过**where语句中**的关联子查询获得。

```sql
select s1.emp_no, s1.salary, (
    select count(distinct s2.salary)
    from salaries s2
    where s2.to_date = '9999-01-01'
    and s1.salary <= s2.salary
)  rank
from salaries s1
where s1.to_date = '9999-01-01'
order by rank, s1.emp_no
```