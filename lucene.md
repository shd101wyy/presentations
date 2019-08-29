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

# Elasticsearch

Elasticsearch 基于 Lucene 进行的封装，写成了 RESTful API 的形式，通过 http 请求就能对其进行操作了。

<!-- slide -->

同时 Elasticsearch 还考虑了海量数据，实现了分布式，是一个可以存储海量数据的分布式搜索引擎

<!-- slide -->

# Elasticsearch 的几个专有名词

- 索引 Index
- 类型 Type
- 文档 Document

<!-- slide -->

# 索引 Index

这个 Index 不是我们之前提到的 Inverted Indexing 的那个 Indexing，而是 Elasticsearch 中的索引存放数据的地方。  
类似于 MySQL 中的一个数据库 (Database)

<!-- slide -->

# 类型 Type

类型是用来定义数据结构的，类似于 MySQL 中的一个表 (Table)

<!-- slide -->

# 文档 Document

文档就是最终的数据，类似于 MySQL 中的行 (Row)

<!-- slide -->

| **Elasticsearch** | **SQL Database** |
| ----------------- | ---------------- |
| 索引 Index        | 数据库 Database  |
| 类型 Type         | 表 Table         |
| 文档 Document     | 行 Row           |

<!-- slide -->

重新以古诗为例。一首诗，有题目、作者、朝代、字数、诗内容等字段，那么首先，我们可以建立一个名叫 Poems 的索引，然后创建一个叫做 Poem 的类型，类型是通过 Mapping 来定义每个字段的类型。

<!-- slide -->

- 索引
  poems

<!-- slide -->

- 类型

```json
"poem": {
  "properties": {
    "title": {
      "type": "keyword"
    },
    "author": {
      "type": "keyword"
    },
    "dynasty": {
      "type": "keyword"
    },
    "words": {
      "type": "integer"
    },
    "content": {
      "type": "text"
    }
  }
}
```

<!-- slide -->

- 文档

```json
{
  "title": "静夜思",
  "author": "李白",
  "dynasty": "唐",
  "words": 20,
  "content": "床前明月光，疑是地上霜。举头望明月，低头思故乡。"
}
```

<!-- slide -->

**keyword** 和 **text** 的区别是什么呢？  
**keyword** 类型是不会分词的，直接根据字符串内容建立反向索引，**text** 类型在存入 Elasticsearch 的时候，会先分词，然后根据分词后的内容建立反向索引。

<!-- slide -->

# Elasticsearch 分布式原理

<!-- slide -->

Elasticsearch 会对数据进行切分，同时每一个分片会保存多个副本，保证了分布式环境下的高可用。

![Screenshot from 2019-08-28 11-58-09](https://i.loli.net/2019/08/28/moLiJ3fAehxsqNR.png)

<!-- slide -->

![Screenshot from 2019-08-28 11-59-51](https://i.loli.net/2019/08/28/de2MxjAyPctJ7mb.png)

建立索引的请求会先发送到 Master，Master 建立完索引后，将集群状态同步至 Slave。同理 Mapping 的建立也是类似的。

注意，只有建立索引和类型需要经过 Master，数据的写入有一个简单的 routing 规则，可以 route 到集群中的任意节点，所以数据的写入压力是分散在整个集群的。

<!-- slide -->

# Lucene

Apache Lucene 是 Elasticsearch 的基石。
Lucene 是基于 Java 编写的开源搜索库，发布于 Apache License 2 下。

<!-- slide -->

搜索引擎涉及到的三个任务，这也是 Lucene 帮助解决的任务：

1. 索引 Indexing：为了有效率的消化和储存数据以便快速地搜索
2. 查询 Querying：使用户可以进行搜索
3. 排名 Ranking：根据用户的需求，展示和排序搜索结果

<!-- slide -->

**Indexing** 就是我们之前提到的 Inverted Indexing 反向索引，
在建立反向索引之前，我们会对文本进行分词，过滤停顿词等。（英文文本还会有 lowercase, stemming 等操作）

<!-- slide -->

**Querying** 解决了如何处理用户输入的搜索，并设立某些布尔规则。

<!-- slide -->

**Ranking** 决定了搜索结果的排序。

例如最传统的 Vector Space Model（VSM）通过比对 Document vector 和 Query vector 的 cosine similarity 来决定搜索结果里文档的排序。

> Document vector 和 Query vector 可以通过 TF-IDF 算得，具体 TF-IDF 是什么大家自己去学习下哈哈

除此之外还有基于概率模型的 BM25 等等用于排序的模型

<!-- slide -->

# Directory

Directory 是 Lucene 存放 inverted indexes（反向索引）的地方。  
它和 Elasticsearch 中的索引 Index 的概念很像。

```java
// Target path where the inverted indexes are stored on the filesystem
Path path = Paths.get("/home/lucene/luceneidx");
// Obtains a read-only view of the search engine via an IndexReader
Directory directory = FSDirectory.open(path);
// Opens a Directory on the target path
IndexReader reader = DirectoryReader.open(directory);
```

<!-- slide -->

# IndexReader

你可以使用 `IndexReader` 来得到一个索引的统计信息，例如我们存了多少个文档（Document，和 Elasticsearch 中的 Document 意义完全一致），或者哪些文档被删除了。  
如果你知道一个文档的 ID，你可以直接通过 IndexReader 得到这个文档。

```java
int ID = 123;
Document document = reader.document(identifier);
```

<!-- slide -->

# QueryParser

你可以使用 QueryParser 来处理用户的搜索输入。在搜索时你需要指定文本分析的方式。在 Lucene 中，文本分析的任务是由 Analyzer API 实现的。一个 Analyzer 由 Tokenizer 和 TokenFilter 组成。当然 Lucene 自身也提供了一些内置的 Analyzer。例如：

```java
// 创建一个 query parser 给 title 这一项，并使用 WhitespaceAnalyzer
QueryParser parser = new QueryParser("title", new WhitespaceAnalyzer());
// 处理用户输入的搜索，并返回一个 Lucene Query
Query query = parser.parse("+Deep +search")
```

<!-- slide -->

# IndexSearcher

接着你可以创建 IndexSearcher 来搜索上一步里面创建的 Query

```java
// 使用 IndexSearcher 对 query 进行搜索，返回最前面的 10 个文档
IndexSearcher searcher = new IndexSearcher(reader);
TopDocs hits = searcher.search(query, 10);

for (int i = 0; i < hits.scoreDocs.length; i++) {
  ScoreDoc scoreDoc = hits.scoreDocs[i];
  Document doc = reader.document(scoreDoc.doc);
  System.out.println(doc.get("title") + ": " + scoreDoc.score);
}
```

<!-- slide -->

# per-field analyzers

和 Elasticsearch 中的 Type （Mappings）很相似

```java
Map<String, Analyzer> perFieldAnalyzers = new HashMap<>();
CharArraySet stopWords = new CharArraySet(Arrays.asList("a", "an", "the"), true);
// 停顿词过滤的 analyzer
perFieldAnalyzers.put("page", new StopAnalyzer(stopWords));
// 空格分词的 analyzer
perFieldAnalyzers.put("title", new WhitespaceAnalyzer());
// EnglishAnalyzer 为默认的 analyzer，当存的数据存在 page 和 title 以外的项时，会调用 EnglishAnalyzer
Analyzer analyzer = new PerFieldAnalyzerWrapper(
  new EnglishAnalyzer(), perFieldAnalyzers));
```

<!-- slide -->

# Document

如何添加 Documents（文档）:

```java
IndexWriterConfig config = new IndexWriterConfig(analyzer);
IndexWriter writer = new IndexWriter(directory, config);

Document d14s = new Document();
d14s.add(new TextField("title", "DL for search", Field.Store.YES));
d14s.add(new TextField("page", "Living in the information age ...", Field.Store.YES));

Document rs = new Document();
rs.add(new TextField("title", "Relevant search", Field.Store.YES));
rs.add(new TextField("page", "Getting a search engine to behave ...", Field.Store.YES));

writer.addDocument(d14s);
writer.addDocument(rs);
```

<!-- slide -->

```java
writer.commit(); // Commits the changes
writer.close(); // Closes the IndexWriter (releases resources)
```

<!-- slide -->

# References

- [终于有人把 Elasticsearch 原理讲透了](https://zhuanlan.zhihu.com/p/62892586)
- Deep Learning for Search
