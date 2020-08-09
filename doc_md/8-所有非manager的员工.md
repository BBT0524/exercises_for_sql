# 1.[sql10 所有非manager的员工emp_no](https://www.nowcoder.com/practice/32c53d06443346f4a2f2ca733c19660c?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

## 题目描述

获取所有非manager的员工emp_no
CREATE TABLE `dept_manager` (
`dept_no` char(4) NOT NULL,
`emp_no` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`dept_no`));
CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,

PRIMARY KEY (`emp_no`))

# 2. 解法

## 2.1 子查询 not  in

主要思想就是集合论中的补集，先通过子查询获取 dept_manger 表中的 emp_no集合， 然后求取该集合在 employees表中的补集。

```sql
select e.emp_no
from employees e
where e.emp_no not in ( select d.emp_no 
                        from dept_manager d)
```

## 2.2 EXCEPT 求补集运算

sql支持集合运算：

- except：求补集
- union：求并集
- intersect：求交集

```sql
select e.emp_no
from employees e
except
select d.emp_no 
from dept_manager d
```

## 2.3 LEFT JOIN 

将 表 employees 左连接到表 dept_manager， 通过 emp_no对齐。 如果 dept_manager.emp_no 为null， 则该条记录属于非manger员工。其核心还是补集的思想。

```sql
select e.emp_no
from employees e
left join dept_manager d
on e.emp_no = d.emp_no
where d.emp_no is null
```

