# [sql01 题目描述](https://www.nowcoder.com/practice/218ae58dfdcd4af195fff264e062138f?tpId=82&&tqId=29753&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

查找最晚入职员工的所有信息，为了减轻入门难度，目前所有的数据里员工入职的日期都不是同一天(sqlite里面的注释为--,mysql为comment)
CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL, -- '员工编号'
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));

## 方法1：使用max()函数

```sql
select emp_no, birth_date, first_name, last_name, gender, max(hire_date)
from employees
```

下面来自

博主：北京某知名靓仔

博文链接：https://blog.nowcoder.net/n/e2b2e8b44f8241199f106a4e7cb27455

## 方法2：排序+limit

13ms,  3268Kb

```sql
select *
from employees
order by hire_date desc
limit 1
```

## 方法3：排序 + limit + offset

```sql
select *
from employees
order by hire_date desc
limit 1 offset 0;
```

## 方法4：排序 + limit a，b

 limit a，b: 从第a条记录，向后读取b个

```sql
-- 使用limit关键字 从第0条记录 向后读取一个，也就是第一条记录 
select * 
from employees
order by hire_date desc
limit 0, 1;
```

## 方法5：子查询

### 5.1 比较子查询

带有比较运算符的子查询是指父查询与子查询之间用比较运算符进行连接。当用户能确切知道**子查询返回的是单个值**时，可以用 >、<、=、>=、<=、!=、或<>等比较运算符。

```sql
select *
from employees
where hire_date = (select max(hire_date) 
                   from employees)
```

### 5.2 子查询 in

```sql
select *
from employees
where hire_date in (select max(hire_date) 
                   from employees)
```



