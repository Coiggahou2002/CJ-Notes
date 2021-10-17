# SQL

## 刷题总结

### 按某列值重复次数最多排序

```sql
select tags_id, count(*) as count
from t_blogs_tags
group by tags_id
order by count desc
```

### 第 K 高查询

如下面例子，查询员工信息表中第 4 高的薪水

```sql
select distinct Salary
    from Employee
    order by Salary desc
    limit 1 offset 3
```

查询第 N 高薪水

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  set N = N - 1;
  RETURN (
      # Write your MySQL query statement below.
      select distinct Salary
      from Employee
      order by Salary desc
      limit 1 offset N
  );
END
```



## JOIN

https://www.cnblogs.com/reaptomorrow-flydream/p/8145610.html

### INNER JOIN

根据条件表达式，对两表做笛卡尔积连接

```sql
select column_name
from table1
inner join table2
on
table1.column_name = table2.column_name
```

### LEFT JOIN

可以理解为先做 INNER JOIN 再与左表取交集

### RIGHT JOIN

### FULL JOIN





