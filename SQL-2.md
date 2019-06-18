# 按天统计每天注册用户数

```sql
select date_format(create_date, "%Y-%m-%d") date, count(1) from mh_member group by date desc;
```