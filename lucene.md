<!-- slide -->

# 我先问个问题

在你学习过的古诗中，请说出带 **前** 字的诗句。

<!-- slide -->

- 静夜思

床**前**明月光  
疑是地上霜  
举头望明月  
低头思故乡

<!-- slide -->

- 望庐山瀑布

日照香炉生紫烟  
遥看瀑布挂**前**川  
飞流直下三千尺  
疑是银河落九天

<!-- slide -->

明明都会背诵，但是为什么如果让你们找到含有 **前** 字的古诗，会很难说出来呢？

<!-- slide -->

# 答案

因为我们的脑子里面没有建立**反向索引 (inverted index)**。

<!-- slide -->

# 问题

大家一般背诵古诗都是怎么背诵的呢？

<!-- slide -->

我们脑子中的索引，一般是以诗名作为 Key，诗的内容作为 Value。

例如:

```
Key                     Value

                       床前明月光
静夜思       =>         疑是地上霜
                       举头望明月
                       低头思故乡
```

所以当我让你们背诵静夜思你马上能反应过来，因为你从你的脑子中的索引直接找到了诗。

<!-- slide -->

但是我让你说出带“前”字的诗句，由于没有索引，你只能遍历脑海中所有的诗句，当你的脑海中诗词量大，就很难在短时间内得到结果。

<!-- slide -->

# 反向索引

反向索引 (Inverted Indexing)  
举个例子  
我们以 “**前**” 字作为 Key，诗句作为 Value

```
Key                     Value

前          =>         床前明月光
                       疑是地上霜
```

<!-- slide -->

# 索引爆炸

如果每个字都用来建立反向索引...

```
Key                     Value
床          =>         床前明月光
                       疑是地上霜

前          =>         床前明月光
                       疑是地上霜
...
上          =>         床前明月光
                       疑是地上霜

霜          =>         床前明月光
                       疑是地上霜
```

<!-- 记忆量会爆炸性增长 -->

<!-- slide -->

# 解决方案？

<!-- slide -->

我们可以依旧用诗句中的字作为 Key，但不再使用诗句作为 Value，而是使用诗名作为 Value

```
Key                     Value
床          =>          静夜思

前          =>          静夜思

...
上          =>          静夜思

霜          =>          静夜思
```

这样一来 Value 中存的东西就少了

<!-- slide -->

我们还可以将诗句中的字作为 Key，对应多个诗名作为 Value，而不仅仅是一个诗名

<br>

```
Key                     Value
前          =>          静夜思
                        望庐山瀑布
```

<br>

这样在我们想要搜索带有 **前** 字的古诗时，我们可以迅速的找到 **静夜思** 和 **望庐山瀑布** 这两首古诗，然后再从古诗的诗句中，找到含有 **前** 字的诗句

<!-- slide -->

# 搜索引擎？

百度，搜狗，bing，google...

<!-- slide -->

其实搜索引擎都是基于**反向索引**来快速的找到搜索的内容的。

<!-- slide -->

# 搜索引擎的基本流程

1. 内容爬取
2. 分词，并且停顿词过滤
3. 建立反向索引

<!-- slide -->

![Screenshot from 2019-08-28 10-42-31](https://i.loli.net/2019/08/28/Gk13uAgvDoUMTw2.png)

<!-- slide -->

# ElasticSearch

ElasticSearch 基于 Lucene 进行的封装，写成了 RESTful API 的形式，通过 http 请求就能对其进行操作了。

<!-- slide -->

同时 ElasticSearch 还考虑了海量数据，实现了分布式，是一个可以存储海量数据的分布式搜索引擎

<!-- slide -->

# ElasticSearch 的几个专有名词

- 索引 Index
- 类型 Type
- 文档 Document

<!-- slide -->

# 索引 Index

这个 Index 不是我们之前提到的 Inverted Indexing 的那个 Indexing，而是 ElasticSearch 中的索引存放数据的地方。  
类似于 MySQL 中的一个数据库 (Database)

<!-- slide -->

# 类型 Type

类型是用来定义数据结构的，类似于 MySQL 中的一个表 (Table)

<!-- slide -->

# 文档 Document

文档就是最终的数据，类似于 MySQL 中的行 (Row)

<!-- slide -->

| **ElasticSearch** | **SQL Database** |
| ----------------- | ---------------- |
| 索引 Index        | 数据库 Database  |
| 类型 Type         | 表 Table         |
| 文档 Document     | 行 Row           |

<!-- slide -->

# References

- [终于有人把 ElasticSearch 原理讲透了](https://zhuanlan.zhihu.com/p/62892586)
