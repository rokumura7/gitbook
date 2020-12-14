# 外部キー解除

```sql
-- FKのID確認
> SHOW CREATE TABLE tableA;

> ALTER TABLE tableA DROP FOREIGN KEY `fk_id`;
```

see: [FOREIGN KEY Constraints / Dropping Foreign Key Constraints](https://dev.mysql.com/doc/refman/8.0/en/create-table-foreign-keys.html#foreign-key-dropping)

