**数据类型格式化函数：**


   [PostgreSQL](https://so.csdn.net/so/search?q=PostgreSQL&spm=1001.2101.3001.7020)格式化函数提供一套有效的工具用于把各种数据类型(日期/时间、integer、floating point和numeric)转换成格式化的字符串以及反过来从格式化的字符串转换成指定的数据类型。下面列出了这些函数，它们都遵循一个公共的调用习惯：第一个参数是待格式化的值，而第二个是定义输出或输出格式的模板。

| **函数**                        | **返回类型** | **描述**                  | **例子**                                     |
| ------------------------------- | ------------ | ------------------------- | -------------------------------------------- |
| to_char(timestamp, text)        | text         | 把时间戳转换成字串        | to_char(current_timestamp, 'HH12:MI:SS')     |
| to_char(interval, text)         | text         | 把时间间隔转为字串        | to_char(interval '15h 2m 12s', 'HH24:MI:SS') |
| to_char(int, text)              | text         | 把整数转换成字串          | to_char(125, '999')                          |
| to_char(double precision, text) | text         | 把实数/双精度数转换成字串 | to_char(125.8::real, '999D9')                |
| to_char(numeric, text)          | text         | 把numeric转换成字串       | to_char(-125.8, '999D99S')                   |
| to_date(text, text)             | date         | 把字串转换成日期          | to_date('05 Dec 2000', 'DD Mon YYYY')        |
| to_timestamp(text, text)        | timestamp    | 把字串转换成时间戳        | to_timestamp('05 Dec 2000', 'DD Mon YYYY')   |
| to_timestamp(double)            | timestamp    | 把UNIX纪元转换成时间戳    | to_timestamp(200120400)                      |
| to_number(text, text)           | numeric      | 把字串转换成numeric       | to_number('12,454.8-', '99G999D9S')          |

   \1. 用于日期/时间格式化的模式：

| **模式** | **描述**                                     |
| -------- | -------------------------------------------- |
| HH       | 一天的小时数(01-12)                          |
| HH12     | 一天的小时数(01-12)                          |
| HH24     | 一天的小时数(00-23)                          |
| MI       | 分钟(00-59)                                  |
| SS       | 秒(00-59)                                    |
| MS       | 毫秒(000-999)                                |
| US       | 微秒(000000-999999)                          |
| AM       | 正午标识(大写)                               |
| Y,YYY    | 带逗号的年(4和更多位)                        |
| YYYY     | 年(4和更多位)                                |
| YYY      | 年的后三位                                   |
| YY       | 年的后两位                                   |
| Y        | 年的最后一位                                 |
| MONTH    | 全长大写月份名(空白填充为9字符)              |
| Month    | 全长混合大小写月份名(空白填充为9字符)        |
| month    | 全长小写月份名(空白填充为9字符)              |
| MON      | 大写缩写月份名(3字符)                        |
| Mon      | 缩写混合大小写月份名(3字符)                  |
| mon      | 小写缩写月份名(3字符)                        |
| MM       | 月份号(01-12)                                |
| DAY      | 全长大写日期名(空白填充为9字符)              |
| Day      | 全长混合大小写日期名(空白填充为9字符)        |
| day      | 全长小写日期名(空白填充为9字符)              |
| DY       | 缩写大写日期名(3字符)                        |
| Dy       | 缩写混合大小写日期名(3字符)                  |
| dy       | 缩写小写日期名(3字符)                        |
| DDD      | 一年里的日子(001-366)                        |
| DD       | 一个月里的日子(01-31)                        |
| D        | 一周里的日子(1-7；周日是1)                   |
| W        | 一个月里的周数(1-5)(第一周从该月第一天开始)  |
| WW       | 一年里的周数(1-53)(第一周从该年的第一天开始) |

   \2. 用于数值格式化的模板模式：

| **模式** | **描述**                         |
| -------- | -------------------------------- |
| 9        | 带有指定数值位数的值             |
| 0        | 带前导零的值                     |
| .(句点)  | 小数点                           |
| ,(逗号)  | 分组(千)分隔符                   |
| PR       | 尖括号内负值                     |
| S        | 带符号的数值                     |
| L        | 货币符号                         |
| D        | 小数点                           |
| G        | 分组分隔符                       |
| MI       | 在指明的位置的负号(如果数字 < 0) |
| PL       | 在指明的位置的正号(如果数字 > 0) |
| SG       | 在指明的位置的正/负号            |


**八、时间/日期函数和操作符：**

   \1. 下面是PostgreSQL中支持的时间/日期操作符的列表：

| **操作符** | **例子**                                                    | **结果**                     |
| ---------- | ----------------------------------------------------------- | ---------------------------- |
| +          | date '2001-09-28' + integer '7'                             | date '2001-10-05'            |
| +          | date '2001-09-28' + interval '1 hour'                       | timestamp '2001-09-28 01:00' |
| +          | date '2001-09-28' + time '03:00'                            | timestamp '2001-09-28 03:00' |
| +          | interval '1 day' + interval '1 hour'                        | interval '1 day 01:00'       |
| +          | timestamp '2001-09-28 01:00' + interval '23 hours'          | timestamp '2001-09-29 00:00' |
| +          | time '01:00' + interval '3 hours'                           | time '04:00'                 |
| -          | - interval '23 hours'                                       | interval '-23:00'            |
| -          | date '2001-10-01' - date '2001-09-28'                       | integer '3'                  |
| -          | date '2001-10-01' - integer '7'                             | date '2001-09-24'            |
| -          | date '2001-09-28' - interval '1 hour'                       | timestamp '2001-09-27 23:00' |
| -          | time '05:00' - time '03:00'                                 | interval '02:00'             |
| -          | time '05:00' - interval '2 hours'                           | time '03:00'                 |
| -          | timestamp '2001-09-28 23:00' - interval '23 hours'          | timestamp '2001-09-28 00:00' |
| -          | interval '1 day' - interval '1 hour'                        | interval '23:00'             |
| -          | timestamp '2001-09-29 03:00' - timestamp '2001-09-27 12:00' | interval '1 day 15:00'       |
| *          | interval '1 hour' * double precision '3.5'                  | interval '03:30'             |
| /          | interval '1 hour' / double precision '1.5'                  | interval '00:40'             |

  \2. 日期/时间函数：

| **函数**                      | **返回类型** | **描述**                                     | **例子**                                            | **结果**                |
| ----------------------------- | ------------ | -------------------------------------------- | --------------------------------------------------- | ----------------------- |
| age(timestamp, timestamp)     | interval     | 减去参数，生成一个使用年、月的"符号化"的结果 | age('2001-04-10', timestamp '1957-06-13')           | 43 years 9 mons 27 days |
| age(timestamp)                | interval     | 从current_date减去得到的数值                 | age(timestamp '1957-06-13')                         | 43 years 8 mons 3 days  |
| current_date                  | date         | 今天的日期                                   |                                                     |                         |
| current_time                  | time         | 现在的时间                                   |                                                     |                         |
| current_timestamp             | timestamp    | 日期和时间                                   |                                                     |                         |
| date_part(text, timestamp)    | double       | 获取子域(等效于extract)                      | date_part('hour', timestamp '2001-02-16 20:38:40')  | 20                      |
| date_part(text, interval)     | double       | 获取子域(等效于extract)                      | date_part('month', interval '2 years 3 months')     | 3                       |
| date_trunc(text, timestamp)   | timestamp    | 截断成指定的精度                             | date_trunc('hour', timestamp '2001-02-16 20:38:40') | 2001-02-16 20:00:00+00  |
| extract(field from timestamp) | double       | 获取子域                                     | extract(hour from timestamp '2001-02-16 20:38:40')  | 20                      |
| extract(field from interval)  | double       | 获取子域                                     | extract(month from interval '2 years 3 months')     | 3                       |
| localtime                     | time         | 今日的时间                                   |                                                     |                         |
| localtimestamp                | timestamp    | 日期和时间                                   |                                                     |                         |
| now()                         | timestamp    | 当前的日期和时间(等效于 current_timestamp)   |                                                     |                         |
| timeofday()                   | text         | 当前日期和时间                               |                                                     |                         |

  \3. EXTRACT，date_part函数支持的field：

| **域**       | **描述**                                                     | **例子**                                                  | **结果** |
| ------------ | ------------------------------------------------------------ | --------------------------------------------------------- | -------- |
| CENTURY      | 世纪                                                         | EXTRACT(CENTURY FROM TIMESTAMP '2000-12-16 12:21:13');    | 20       |
| DAY          | (月分)里的日期域(1-31)                                       | EXTRACT(DAY from TIMESTAMP '2001-02-16 20:38:40');        | 16       |
| DECADE       | 年份域除以10                                                 | EXTRACT(DECADE from TIMESTAMP '2001-02-16 20:38:40');     | 200      |
| DOW          | 每周的星期号(0-6；星期天是0) (仅用于timestamp)               | EXTRACT(DOW FROM TIMESTAMP '2001-02-16 20:38:40');        | 5        |
| DOY          | 一年的第几天(1 -365/366) (仅用于 timestamp)                  | EXTRACT(DOY from TIMESTAMP '2001-02-16 20:38:40');        | 47       |
| HOUR         | 小时域(0-23)                                                 | EXTRACT(HOUR from TIMESTAMP '2001-02-16 20:38:40');       | 20       |
| MICROSECONDS | 秒域，包括小数部分，乘以 1,000,000。                         | EXTRACT(MICROSECONDS from TIME '17:12:28.5');             | 28500000 |
| MILLENNIUM   | 千年                                                         | EXTRACT(MILLENNIUM from TIMESTAMP '2001-02-16 20:38:40'); | 3        |
| MILLISECONDS | 秒域，包括小数部分，乘以 1000。                              | EXTRACT(MILLISECONDS from TIME '17:12:28.5');             | 28500    |
| MINUTE       | 分钟域(0-59)                                                 | EXTRACT(MINUTE from TIMESTAMP '2001-02-16 20:38:40');     | 38       |
| MONTH        | 对于timestamp数值，它是一年里的月份数(1-12)；对于interval数值，它是月的数目，然后对12取模(0-11) | EXTRACT(MONTH from TIMESTAMP '2001-02-16 20:38:40');      | 2        |
| QUARTER      | 该天所在的该年的季度(1-4)(仅用于 timestamp)                  | EXTRACT(QUARTER from TIMESTAMP '2001-02-16 20:38:40');    | 1        |
| SECOND       | 秒域，包括小数部分(0-59[1])                                  | EXTRACT(SECOND from TIMESTAMP '2001-02-16 20:38:40');     | 40       |
| WEEK         | 该天在所在的年份里是第几周。                                 | EXTRACT(WEEK from TIMESTAMP '2001-02-16 20:38:40');       | 7        |
| YEAR         | 年份域                                                       | EXTRACT(YEAR from TIMESTAMP '2001-02-16 20:38:40');       | 2001     |

  \4. 当前日期/时间：
   我们可以使用下面的函数获取当前的日期和/或时间∶
   CURRENT_DATE
   CURRENT_TIME
   CURRENT_TIMESTAMP
   CURRENT_TIME (precision)
   CURRENT_TIMESTAMP (precision)
   LOCALTIME
   LOCALTIMESTAMP
   LOCALTIME (precision)
   LOCALTIMESTAMP (precision)