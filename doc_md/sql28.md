<style>
/*隔行变色*/
table tbody tr:nth-child(2n) {
    background: rgba(158,188,226,0.12); 
}

/*悬浮变色：鼠标指向表格内该行时，该行变色*/
table tr:hover {
    background: #efefef; 
}
</style>

# [sql28](https://www.nowcoder.com/practice/3a303a39cc40489b99a7e1867e6507c5?tpId=82&&tqId=29780&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)

## 1. 题目描述

film表
字段|	说明
---|---
film_id	|电影id
title	|电影名称
description|	电影描述信息

```sql
CREATE TABLE IF NOT EXISTS film (
film_id smallint(5)  NOT NULL DEFAULT '0',
title varchar(255) NOT NULL,
description text,
PRIMARY KEY (film_id));
```

category表

字段 | 说明
---|---
category_id	|电影分类id
name	|电影分类名称
last_update|电影分类最后更新时间

```sql
CREATE TABLE category  (
category_id  tinyint(3)  NOT NULL ,
name  varchar(25) NOT NULL, `last_update` timestamp,
PRIMARY KEY ( category_id ));
```

film_category表
字段|说明
---|---
film_id|电影id
category_id|电影分类id
last_update	|电影id和分类id对应关系的最后更新时间

```sql
CREATE TABLE film_category  (
film_id  smallint(5)  NOT NULL,
category_id  tinyint(3)  NOT NULL, `last_update` timestamp);
```

查找描述信息(film.description)中包含robot的电影对应的分类名称(category.name)以及电影数目(count(film.film_id))，而且还需要该分类包含电影总数量(count(film_category.category_id))>=5部


## 2. 解法

题目理解：查找电影描述中带有roobt的记录信息。要求查询其分类名称和电影数目。但是对电影所在的分类有要求，要求该分类的电影数目要大于4部，必须要注意的是，这个数量要求不是针对roobt的。

### 2.1 内连接+通配符+聚合

1. 首先通过内连接形成一个扩充信息表：film_id |description | category_id |name
也就是每个film都扩充了其对应的category_id(count(category_id)>=5)和类别name的信息。
2. 对上面这个扩充信息表，查询film.film_id的description中匹配'%robot%'的电影数量，及其对应的电影类别名称。

```sql
select c.name, count(f.film_id)
from film f
-- (film_id, desc) -(fc)-> category_id -(c)-> name
join film_category fc on f.film_id = fc.film_id
join category c  on c.category_id = fc.category_id  
and c.category_id in (
    select category_id
    from film_category 
    group by category_id
    having count(film_id) >= 5
)
where f.description like '%robot%'
```

### 2.2 用where代替2.1中的内连接

```sql
select c.name, count(f.film_id)
from film f, film_category fc, category c
-- (film_id, desc) -(fc)-> category_id -(c)-> name
where f.film_id = fc.film_id
and c.category_id = fc.category_id  
and c.category_id in (
    select category_id
    from film_category 
    group by category_id
    having count(film_id) >= 5
)
and f.description like '%robot%'
```

## 3. SQL的其他通配符

原始的表 (用在例子中的)：
Persons 表:

Id|	LastName|	FirstName|	Address	|City
---|---|---|---|---
1|	Adams|	John|	Oxford Street|	London
2|	Bush|	George|	Fifth Avenue|	New York
3|	Carter|	Thomas|	Changan Street|	Beijing

### 3.1 SQL通配符

表1：SQL通配符说明
<table>
    <thead>
        <th> 通配符 </th>
        <th> 例  子 </th>
        <th> 说  明 </th>
    </thead>
    <tbody>
        <tr>
            <td rowspan='2'> % </td>
            <td> WHERE City LIKE 'Ne%'</td>
            <td> 匹配以 "Ne" 开始的城市，匹配结果为 New York所在的记录项id=2</td>
        </tr>
        <tr>
            <td>WHERE City LIKE '%lond%' </td>
            <td>匹配包含 "lond" 的城市， 匹配结果为London 所在的记录项id=1</td>
        </tr>
        <tr>
            <td rowspan='2'> _ </td>
            <td> WHERE FirstName LIKE '_eorge' </td>
            <td> 匹配第一个字符之后是 "eorge"的名字，匹配结果是George所在的记录项id=2</td>
        </tr>
        <tr>
            <td>WHERE FirstName LIKE 'C_r_er' </td>
            <td> 以 "C" 开头，然后是一个任意字符，然后是 "r"，然后是任意字符，然后是 "er"的姓氏，匹配结果是Carter所在的记录项id=3</td>
        </tr>
        <tr>
            <td rowspan='2'> [charlist] </td>
            <td>WHERE City LIKE '[ALN]%' </td>
            <td> 匹配以 "A" 或 "L" 或 "N" 开头的城市，匹配结果是London、New York所在的记录项id=1和2</td>
        </tr>
        <tr>
            <td>可以同%和_联合使用，用以缩小匹配字符的范围</td>
            <td> '%[er]%'等</td>
        </tr>
        <tr>
            <td rowspan='2'> [!charlist]</td>
            <td>WHERE City LIKE '[!ALN]%' </td>
            <td> 不以 "A" 或 "L" 或 "N" 开头的城市，匹配结果是Beijing所在的记录项id=3</td>
        </tr>
        <tr>
            <td> 同[charlist]，也可同%和_联合使用 </td>
            <td> '%[!er]%'等</td>
        </tr>
    </tbody>
</table>

### 3.2 通配符注意事项

1. 复合搜索时，尽量将通配符写在搜索语句的后面，因为通配符的搜索时间比较长。
2. % 匹配任意次数
3. % 不能匹配null值


### 3.3 正则表达式

正则表达式用于更加复杂的匹配场景。但是sqlite不支持。mysql等支持正则表达式。相关函数为REGEXP等。


## 参考
- https://www.w3school.com.cn/sql/sql_wildcards.asp