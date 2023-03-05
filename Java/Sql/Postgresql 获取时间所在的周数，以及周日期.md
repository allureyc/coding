获取周数

```sql
SELECT date_part('week',TIMESTAMP '2021-03-11');
```


获取周时间

```sql
SELECT date_trunc('week', '2021-03-11'::timestamp);
```

如果你不想获取周起始时间，也可以获取结束时间

```sql
SELECT    date_trunc('week', '2012-07-25 22:24:22'::timestamp)::date
   || ' '
   || (date_trunc('week', '2012-07-25 22:24:22'::timestamp)+ '6 days'::interval)::date;

```

