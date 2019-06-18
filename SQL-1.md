# 按照一定的条件给用户批量添加优惠券

```sql
insert into p2p_interest_coupon_code(create_date,modify_date,code, used, interest_coupon, member,end_date) 
select now(),now(),concat("C20171026C00000000000000000", 10000+id),0,19,id,"2017-10-26 23:59:59" from p2p_member where is_borrowing = 0
```
