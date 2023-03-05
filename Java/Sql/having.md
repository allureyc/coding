# Having

having称为分组滤过条件,也就是说是分组需要的条件,所以必须与group by联用
也就是说，聚合函数计算的结果可以当条件来使用，因为它无法放在where里，只能通过having这种方式来解决。

聚合函数通过作用于一组数据而只返回一个单个值
WHERE关键字不能处理一个数值大小的比较，而HAVING语句可以
故HAVING语句的存在弥补了WHERE关键字不能与聚合函数联合使用的不足。

HAVING语句通常与GROUP BY语句联合使用，用来过滤由GROUP BY语句返回的记录集。

HAVING语句的存在弥补了WHERE关键字不能与聚合函数联合使用的不足。

语法：

```sql
SELECT column1, column2, ... column_n, aggregate_function (expression)
FROM tables
WHERE predicates
GROUP BY column1, column2, ... column_n
HAVING condition1 ... condition_n;
```

同样使用本文中的学生表格，如果想查询平均分高于80分的学生记录可以这样写：

```sql
SELECT id, COUNT(course) as numcourse, AVG(score) as avgscore

FROM student

GROUP BY id

HAVING AVG(score)>=80;
```

在这里，如果用WHERE代替HAVING就会出错。

小注：
SQL语言中设定集合函数的查询条件时使用HAVING从句而不是WHERE从句。通常情况下，HAVING从句被放置在SQL命令的结尾处。