# 多条件排序：从小到大排序，但 0 或者 NULL 排在最后

我们都知道，通过 ```order by {字段名} [asc | desc]``` 可以完成查询结果的排序，
但是有一天遇到一个特殊的需求，按照排序字段升序，但是 0 排在最后，这种情况我们我们应该怎样处理呢？

## 这个是普通排序效果，我们可以看到 0 在最前面
![](https://img.xiaoi.me/pms-upload/20190618/a6e09239-4cdf-022c-1e42-34c637230b0d.png)

## 我们接下来进行简单的处理，看看效果
![](https://img.xiaoi.me/pms-upload/20190618/2ade5634-1bf1-9e76-cc47-1081e6be43e9.png)

```
select * from test order by seq=0,seq;
```

0 和 NULL 都排在后面
```
select * from sql_3 order by seq=0 or seq is null,seq;
```