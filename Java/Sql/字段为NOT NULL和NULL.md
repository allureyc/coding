# SQL_修改字段为NOT NULL和NULL

设置不可为空

```SQL
alter table 【表名】 alter 【字段名】 set not null;

alter table jcz alter address set not null;
```

设置可为空

```sql
alter table 【表名】 alter 【字段名】 drop not null;

alter table jcz alter address drop not null;
```

【若字段中数据存在空值，该字段无法从 NULL 改为 NOT NULL 】

