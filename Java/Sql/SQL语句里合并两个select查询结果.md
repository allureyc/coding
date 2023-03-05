# [SQL语句里合并两个select查询结果](https://www.cnblogs.com/apolloren/p/10469208.html)

SQL [UNION](https://www.baidu.com/s?wd=UNION&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) 操作符
[UNION](https://www.baidu.com/s?wd=UNION&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) 操作符用于合并两个或多个 [SELECT](https://www.baidu.com/s?wd=SELECT&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) 语句的结果集。

请注意，[UNION](https://www.baidu.com/s?wd=UNION&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) 内部的 [SELECT](https://www.baidu.com/s?wd=SELECT&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 [SELECT](https://www.baidu.com/s?wd=SELECT&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) 语句中的列的顺序必须相同。

SQL UNION 语法
SELECT column_name(s) [FROM](https://www.baidu.com/s?wd=FROM&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) table_name1
UNION
SELECT column_name(s) [FROM](https://www.baidu.com/s?wd=FROM&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) table_name2

默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

SQL UNION ALL 语法
SELECT column_name(s) [FROM](https://www.baidu.com/s?wd=FROM&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) table_name1
UNION ALL
SELECT column_name(s) FROM table_name2
另外，UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。

你可以去这个网址看看，里面有更详细的示例．
http://www.w3school.com.cn/sql/sql_union.asp