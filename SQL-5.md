# 反向 like 查询，匹配一段话中是否包含关键字、敏感词

我们一般使用 like 查询都是查询数据库中是否有包含关键字的数据记录，
其实 like 也可以反过来使用，我们将关键字录入到数据库中，
查询的时候检查我们的一段话中是否存在数据库中的关键字，
我能想到的应用场景大概有：

 * 根据用户输入的内容自动匹配关键字对应回复
 * 检查用户上传的内容中是否有包含禁止类敏感词

常规的 like 查询
![](https://img.xiaoi.me/pms-upload/20190618/9489dafc-f91e-6d1d-4bf9-d5069fae9f71.png)

反向 like 查询
![](https://img.xiaoi.me/pms-upload/20190618/ba7b4d34-d02f-ea73-ca5a-85a8f280406e.png)

```sql
SELECT id,keywords FROM sql_5 WHERE '这里是一个有天猫和京东商城的聚合类网站'  LIKE CONCAT('%',keywords,'%');
```
