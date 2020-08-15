# [sql32](https://www.nowcoder.com/practice/6744b90bbdde40209f8ecaac0b0516fe?tpId=82&&tqId=29800&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

## 1. 题目描述

将employees表的所有员工的last_name和first_name拼接起来作为Name，中间以一个空格区分
(注：该数据库系统是sqllite,字符串拼接为 || 符号，不支持concat函数)

```sql
CREATE TABLE `employees` ( `emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));
```

## 2. 解法

不同类型的数据库操作不同

### 2.1 sqlite：||

sqlite的字符串连接方法是 ||

```sql
select (last_name ||" "|| first_name) name
from employees

```

### 2.2 MySQL、SQL Server、Oracle: contact()

```sql
select CONCAT(last_name," "，first_name)  name  
from employees
```
