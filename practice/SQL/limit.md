## limit 问题
MySQL的limit给分页带来了极大的方便，但数据量一大的时候，limit的性能就急剧下降

````sql
SELECT * FROM `will_test` limit 10000,10 
　　
SELECT * FROM `will_test` limit 0,10
````
以上两个数据就不是一个量级的, 第一个语句要先找出 10000 行后再取10行, 所以速度很慢

解决办法:
> 记录每次取出后的最大id 然后 where id = 最大id limit 10；这样就可以解决了 `SELECT * FROM will_test where id = 10000 limit 10 `
>
> 或者限制分页得数量, 每次只能翻100页、超过一百页的需要重新加载后面的100页

````
如果对于有 where 条件，又想走索引用 limit 的，必须设计一个索引，将where 放第一位，limit用到的主键放第2位，而且只能select 主键！
````

````sql
SELECT * FROM `firmware_whole` limit 2 offset 1 # limit后面跟的是2条数据，offset后面是从第1条开始读取。

SELECT * FROM `firmware_whole` limit 2, 1 # limit后面是从第2条开始读，读取1条信息。
````
