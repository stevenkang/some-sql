# some-sql
一些工作过程中的 SQL

# 【根据查询结果update】
```sql
update A表 a INNER JOIN(select 字段1,字段2 from B表 b)t on t.字段2 = a.字段2 set a.字段3 = "XXX";
```
格式：update table_a inner join table_b on xxx = xxx set table_a.xxx = "xxx"

# 【反向like查询】
```sql
SELECT id,keywords FROM wx_keywords WHERE '百度一下'  LIKE CONCAT('%',keywords,'%');
```

# 【字段数据转移】
```sql
update Devices as A left join Devices as B on A.ParentCode = B.Code set A.ParentID = B.ID;
```

# 【区分大小写查询】
```sql
select * from wx_news where url like binary("%appid%");
```

# 【时间偏移】
时间字段 <= date_add(now(), interval - 6 MONTH)
 > 参考：http://www.w3school.com.cn/sql/func_date_add.asp

# MySQL 添加索引
```
1.添加PRIMARY KEY（主键索引）   www.2cto.com  
 
mysql>ALTER TABLE `table_name` ADD PRIMARY KEY ( `column` )
 
2.添加UNIQUE(唯一索引)
 
mysql>ALTER TABLE `table_name` ADD UNIQUE (
 
`column`
)
 
3.添加INDEX(普通索引)
mysql>ALTER TABLE `table_name` ADD INDEX index_name ( `column` )
 
 
4.添加FULLTEXT(全文索引)
mysql>ALTER TABLE `table_name` ADD FULLTEXT ( `column`)
 
5.添加多列索引
mysql>ALTER TABLE `table_name` ADD INDEX index_name ( `column1`, `column2`, `column3` )
```

# 【从小到大排序，0在最后】
```sql
select * from test order by seq=0,seq;
```

# 【MySQL计算时间差】
```sql
SELECT TIMESTAMPDIFF(MONTH,'2009-10-01','2009-09-01');
```
interval可是：SECOND 秒、MINUTE 分钟、HOUR、DAY 天、MONTH 月、YEAR 年 

# 【按月统计数据】
```sql
select DATE_FORMAT(create_date,'%Y年%m月') months,sum(amount) amount from p2p_investment where transfer is null group by months;
```
 > 参考网址：http://linkyou.blog.51cto.com/1332494/751980/

# 【替换URL域名】
```sql
update p2p_ad set url = concat("https://www.51checai.com/", substr(url,LENGTH("http://test.51checai.com/")+1)) where url like "http://test.51checai.com/%";
update `p2p_borrowing_material`  set large = concat("https://img-51checai.b0.upaiyun.com/", substr(large,LENGTH("https://img.51checai.com/")+1)) where large like "https://img.51checai.com/%";
```

# 【推荐数据修复】
查询：
```sql
select id,create_date,`referral_setting`,date_add(create_date, interval +180 day),
CONCAT("{\"referralAmount\":10000,\"referralGift\":\"31\",\"referralGiftTwo\":\"32,33\",\"paymentTime\":\"2016-04-01\",\"finishTime\":\""
       ,DATE_FORMAT(date_add(create_date, interval +180 day),'%Y-%m-%d')
       ,"\",\"feeRate\":0.01}")
from p2p_member where create_date >= "2016-04-01" and create_date < "2016-09-22" and `referrer` is not null order by id desc;
```
更新：
```sql
update p2p_member set `referral_setting` = 
 CONCAT("{\"referralAmount\":10000,\"referralGift\":\"31,38\",\"referralGiftTwo\":\"32,33,39\",\"paymentTime\":\"2016-04-01\",\"finishTime\":\""
       ,DATE_FORMAT(date_add(create_date, interval +180 day),'%Y-%m-%d'),"\",\"feeRate\":0.01}")
where create_date >= "2016-09-22" and `referrer` is not null;
```

# 【指定数据互换】
![](https://img.xiaoi.me/pms-upload/20190618/0434b9b0-c317-ab97-b6bf-5f40ab2e31a3.jpg)
```sql
update test t1 
left join test t2 on t1.id = ((select 1+2)-t2.id )
set t1.position = t2.position 
where t1.id in (1,2);
```

# 一条不知道做了什么的 SQL
```sql
insert into web_statis(user,create_time,stone,dirt,orecoal,ironore,goldore,diamond,allore) 
select al.*,(stone+dirt+orecoal+ironore+goldore+diamond) as allore from (select cu.user,
'2015-2-9 10:09:18' as create_time,(case when c1 is null then 0 else c1 end) as stone,(case when c2 is null then 0 else c2 end) as dirt,(case when c3 is null then 0 else c3 end) as orecoal
,(case when c4 is null then 0 else c4 end) as ironore,(case when c5 is null then 0 else c5 end) as goldore,(case when c6 is null then 0 else c6 end) as diamond from co_user cu
 left join (select count(*) c1,user from co_block where type in(1,4) and time >=1423353600 and time < 1423440000 group by user) tc1 on cu.rowid = tc1.user
 left join (select count(*) c2,user from co_block where type in(2,3) and time >=1423353600 and time < 1423440000 group by user) tc2 on cu.rowid = tc2.user
 left join (select count(*) c3,user from co_block where type = 16 and time >=1423353600 and time < 1423440000 group by user) tc3 on cu.rowid = tc3.user
 left join (select count(*) c4,user from co_block where type = 15 and time >=1423353600 and time < 1423440000 group by user) tc4 on cu.rowid = tc4.user
 left join (select count(*) c5,user from co_block where type = 14 and time >=1423353600 and time < 1423440000 group by user) tc5 on cu.rowid = tc5.user
 left join (select count(*) c6,user from co_block where type = 56 and time >=1423353600 and time < 1423440000 group by user) tc6 on cu.rowid = tc6.user) al
```
