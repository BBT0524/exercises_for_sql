# 1.[sql18](https://www.nowcoder.com/practice/c1472daba75d4635b7f8540b837cc719?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

## 题目描述

查找当前薪水(to_date='9999-01-01')排名第二多的员工编号emp_no、薪水salary、last_name以及first_name，你可以不使用order by完成吗
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

## 2.1 比较子查询+max() : 找出第二大的salary

```sql
select e.emp_no, s.salary, e.last_name, e.first_name
from employees e
join salaries s
on s.emp_no = e.emp_no
and s.to_date = '9999-01-01'
where s.salary = ( 
    select max(salary)
    from salaries
    where salary <  ( 
        select max(salary)
        from salaries
        where to_date = '9999-01-01'
    )
    and to_date = '9999-01-01'
)
```



## 2.2 子查询 + 自连接 ：通过自连接确定第二大salary，并由子查询返回

对表s而言：

|  s   |
| :--: |
| 100  |
|  98  |
|  98  |
|  95  |

**如果以 s1.salary = s2.salary **自连接是这样:

|  s1  |  s2  |
| :--: | :--: |
| 100  | 100  |
|  98  |  98  |
|  98  |  98  |
|  95  |  95  |

本题中是以**s1.salary <= s2.salary**自l连接：即对 表s1中每一条记录项连接到比自身salary大的s2记录项中去，如下表所示： 

|   s1    |   s2    |
| :-----: | :-----: |
| **100** | **100** |
|   98    |   100   |
|   98    |   98    |
|   98    |   98    |
| **98**  | **100** |
| **98**  | **98**  |
| **98**  | **98**  |
|   95    |   100   |
|   95    |   98    |
|   95    |   98    |
|   95    |   95    |

在s1.salary的每个分组内，对s2.salary去重并统计数量m, m就是分组键s1.salary对应的排名。

更多的自连接可参考 https://zhuanlan.zhihu.com/p/141083771

所以, 通过上述s1.salary = s2.salary自连接方式，统计s1.salary的每个分组内，s2.salary去重并统计数量为2的记录项，便得到s1.salary排名为2的记录项，代码如下：

```sql
select e.emp_no, s.salary, e.last_name, e.first_name
from employees e
join salaries s
on s.emp_no = e.emp_no
and s.to_date = '9999-01-01'
where s.salary = ( 
    select s1.salary
    from salaries s1
    join salaries s2
    on s1.salary <= s2.salary
    and s1.to_date = '9999-01-01'
    and s2.to_date = '9999-01-01'
    group by s1.salary
    having count(distinct s2.salary) = 2
)
```



## 2.3 使用order by的版本

```sql
select e.emp_no, s.salary, e.last_name, e.first_name
from employees e
join salaries s
on s.emp_no = e.emp_no
and s.to_date = '9999-01-01'
order by s.salary desc
limit 1, 1
```

# 参考

- https://zhuanlan.zhihu.com/p/141083771

  