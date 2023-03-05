# SQL查询结果把两个字段拼接到一起



举例：从人员基本信息表user_info中查出姓名和毕业学校，把姓名和学校两个字段拼接到一起。

SQL语句：

```sql
select user_name || school as info from user_info;
```

注：拼接字段的符号为管道操作符 | |



concat(fild_name,'_',fild_name) ()将两个字符用_隔开拼接) 才是王道,pgsql和mysql都可以通用



# SQL 怎么取 字符串的前几位

sql中，使用LEFT函数即可取到字符串的前几位。

LEFT(c, number_of_char)用于返回某个被请求的文本域的左侧部分，其中c代表被请求的文本域，number_of_cha代表需要取出的字符串位数。如“LEFT("zhidao.baidu.com", 6)”即可取得字符串"zhidao"。

**扩展资料：**

sql中，常用函数介绍：

1、AVG()：返回平均值

2、COUNT()：返回行数

3、FIRST()：返回第一个记录的值

4、LAST()：返回最后一个记录的值

5、MAX()：返回最大值

6、MIN()：返回最小值

7、SUM()：返回总和

8、UCASE()：将某个字段转换为大写

9、LCASE()：将某个字段转换为小写

10、MID()：从某个文本字段提取字符

11、LEN()：返回某个文本字段的长度

12、ROUND()：对某个数值字段进行指定小数位数的四舍五入

13、NOW()：返回当前的系统日期和时间

14、FORMAT()：格式化某个字段的显示方式

15、INSTR()：返回在某个文本域中指定字符的数值位置

16、LEFT()：返回某个被请求的文本域的左侧部分

17、RIGHT()：返回某个被请求的文本域的右侧部分

