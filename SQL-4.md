# 根据查询结果 update 批量更新

有时候我们需要修复数据或者将 B 表中的数据变更到 A 表中，这个时候我们可以编写一个根据查询结果进行批量更新的 SQL

```sql
update TABLE_A a left join (
  select FIELD_1, FIELD_2, TABLE_A_ID from TABLE_B
) b on b.TABLE_A_ID = a.ID
set a.FIELD_1 = b.FIELD_1, a.FIELD_2 = b.FIELD_2
where a.FIELD_1 is null and a.FIELD_2 is null;
```

以上的 SQL 语句中，我们通过一个子查询先查询 B 表中的数据以及和 A 表的关联字段 ```TABLE_A_ID```，
然后我们使用 left join 进行关联，接着使用 set 命令完成 B 表中的数据批量更新到 A 表，
最后我们使用 where 条件进行限制，防止影响 A 表中有数据的记录