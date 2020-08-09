# [sql02 题目描述](https://www.nowcoder.com/practice/ec1ca44c62c14ceb990c3c40def1ec6c?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

查找入职员工时间排名倒数第三的员工所有信息，为了减轻入门难度，目前所有的数据里员工入职的日期都不是同一天
CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));

## 方法1：排序 + limit + offset

```sql
select *
from employees
order by hire_date desc
limit 1 offset 2    -- pop 前2个后，取1个
```



## 方法2：排序 + limit a, b

 limit a，b: 从第a条记录，向后读取b个

```sql
select *
from employees
order by hire_date desc
limit 2, 1   -- 从第2条记录，向后读取1个
```



## 方法3：使用没有limit的比较子查询  

因为所有的数据里员工入职的日期都不是同一天， 所以倒数第三晚是单个值，满足比较子查询的返回值是单个的要求。

```sql
select * 
from employees
where  hire_date= ( select min(a.hire_date) 
                    from employees a, employees b
                    where a.emp_no!=b.emp_no and a.hire_date < b.hire_date
                    group by a.emp_no
                    having count(*)<3 )

```



