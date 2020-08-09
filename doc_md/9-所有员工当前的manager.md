# 1.[员工当前的manager](https://www.nowcoder.com/practice/e50d92b8673a440ebdf3a517b5b37d62?tpId=82&tags=&title=&diffculty=0&judgeStatus=0&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

## 题目描述

获取所有员工当前的(dept_manager.to_date='9999-01-01')manager，如果员工是manager的话不显示(也就是如果当前的manager是自己的话结果不显示)。输出结果第一列给出当前员工的emp_no,第二列给出其manager对应的emp_no。
CREATE TABLE `dept_emp` (
`emp_no` int(11) NOT NULL, -- '所有的员工编号'
`dept_no` char(4) NOT NULL, -- '部门编号'
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`dept_no`));
CREATE TABLE `dept_manager` (
`dept_no` char(4) NOT NULL, -- '部门编号'
`emp_no` int(11) NOT NULL, -- '经理编号'
`from_date` date NOT NULL,
`to_date` date NOT NULL,

PRIMARY KEY (`emp_no`,`dept_no`));

# 2. 解法

从集合论的角度来看，整体上来说是求的交集问题， 即返回两个表中有相同  dept_no （交集不部分）的 emp_n。同时，除去 在两表中emp_n都相同的项。

## 2.1 join

```sql
select dem.emp_no, dma.emp_no as manager_no
from dept_emp dem
join dept_manager dma
on dem.dept_no = dma.dept_no
where dem.to_date = '9999-01-01'
      and dma.to_date = '9999-01-01'
      and dem.emp_no != dma.emp_no
```

## 2.2 纯where

```sql
select dem.emp_no, dma.emp_no as manager_no
from dept_emp dem, dept_manager dma
where dem.dept_no = dma.dept_no
      and dem.to_date = '9999-01-01'
      and dma.to_date = '9999-01-01'
      and dem.emp_no != dma.emp_no
```

