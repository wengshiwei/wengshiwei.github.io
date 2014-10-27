---
layout: post
title: Lambda-architecture
category:
- every-day-log
---

常为自己不知道hadoop是何物感到惭愧。准确来说，有很多出场率很高的词语，我不知道**它们是什么**，也不知道**它们用来解决什么问题**。于是索性花了点时间把这几个维基词条看了一下(还有几个，上周看的，已经忘了)

- [hadoop](https://en.wikipedia.org/wiki/Apache_Hadoop)
- [spark](https://en.wikipedia.org/wiki/Apache_Spark)

之后又翻到了Nathan marz的[blog](http://nathanmarz.com/) 这位仁兄是Storm的创始人。我看到他在Twitter工作的履历和Stanford的硕士学位，便认为他的东西是可信的。marz在manning上放了新书Big Data - Principles and best practices of scalable realtime data systems的[early access](http://www.manning.com/marz/)，只有第一章可以免费在线阅读。

他在书中提到了一种新的数据处理架构lambda architecture。我发现有个网站就叫[lambda-architecture](http://lambda-architecture.net/)。
下图就是lambda architecture。

![lambda architecture image](http://lambda-architecture.net/img/la-overview_small.png)

1. All data entering the system is dispatched to both the batch layer and the speed layer for processing.
2. The batch layer has two functions: (i) managing the master dataset (an immutable, append-only set of raw data), and (ii) to pre-compute the batch views.
3. The serving layer indexes the batch views so that they can be queried in low-latency, ad-hoc way.
4. The speed layer compensates for the high latency of updates to the serving layer and deals with recent data only.
5. Any incoming query can be answered by merging results from batch views and real-time views.

1是数据源，2、3是off-line的数据处理，4是on-line real time的数据处理，5是最终的查询。

这个架构可以用类比的例子简单的理解。

1. 是世界上的知识
2. 是我们的知识体系、理念、价值观等等
3. 深思熟虑做出的判断。
4. 我们当下的思考
5. 是我们做出的判断

注意2和4的数据源是一样的。同样的知识，我们既能用以事实回答某些问题(1-->4-->5)，也能通过积累事实得出结论做出判断。

再显然一些，比如

1. 今天下雨否
2. 记录下雨，统计每周、每年的下雨量
3. 这个月下了20天雨，这一年有300多天在下雨
4. 连续下了三天的雨
5. 此地雨天多。

这是我今天的想法。既然这个系列叫every-day-log，那么重点是每天的心得记录，而不是记录的准确性。






