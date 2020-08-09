# 1. [sql13 从titles表获取按照title进行分组](https://www.nowcoder.com/practice/72ca694734294dc78f513e147da7821e?tpId=82&&tqId=29765&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

从titles表获取按照title进行分组，每组个数大于等于2，给出title以及对应的数目t。
CREATE TABLE IF NOT EXISTS "titles" (
`emp_no` int(11) NOT NULL,
`title` varchar(50) NOT NULL,
`from_date` date NOT NULL,

`to_date` date DEFAULT NULL);

# 2. 解法

- group by分组

- having控制分组条件

```sql
select title, count(title) t
from titles
group by title
having t > 1
```

