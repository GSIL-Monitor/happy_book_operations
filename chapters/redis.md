# Redis

认为它是一个NO-SQL 数据库也行
认为他是一个 cache 服务器也行。

它里面的存储特点是：
只保存：  key-value.  例如：

'one' =>  1
'two' => 2

在一个有100W数据的表中，如何查到： 姓 “李”并且 性别是 男， 来自于 辽宁的人？

有没有发现： 上面的查询条件中，每个查询条件，都是精确地。 “李”， “男”， “辽宁”， 都是精确的。
（啥叫不精确的呢？   age < 30, age > 20 ,  name like '%思%' )
所以，针对上面的查询，我们可以专门的约定个保存数据的规则：
姓-性别-省份 ,  例如，数据库可以这样保存：

key: 1 , value: '李-男-北京'
key: 2 , value: '李-男-辽宁'
key: 3 , value: '李-女-辽宁'
key: 4 , value: '李-女-北京'

我就可以用极快的速度，查询到结果，应该是：

key: 2 , value: '李-男-辽宁'

它能很好地解决某些对 查询速度很高的 数据库的存储。

一个真实地例子， 浩浩，对于优酷的一个系统，正常的查询，需要 4 个表关联去搜索，每次需要搜索100W个以上的
记录。

当时的问题： 每次页面刷新，都需要3分钟~5分钟。 时间都耗费在 join ... select 上面了。
A X B X C X D = 100 X 100 X 10 X 10

商店表： 15条记录
渠道表： 100个记录
。。另外两个表， 总之相乘后， 100W。

所以，解决办法： 使用REDIS（缓存）。保存时，就直接：

'A-B-C-D' 来保存成key  , 对应的值就是value.

效果： 后来的搜索是： 3~5秒。
