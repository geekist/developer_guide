

## 删除数据库表中的记录

```sql
DELETE FROM table_name [WHERE Clause]
```

如果没有指定 WHERE 子句，MySQL表中的所有记录将被删除。

你可以在 WHERE 子句中指定任何条件

例如：

```
DELETE FROM u_business WHERE `ins_id`=10069
```

您可以在单个表中一次性删除记录。


