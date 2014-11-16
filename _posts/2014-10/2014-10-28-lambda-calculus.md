---
layout: post
title: Lambda Calculus 1 & Others
category:
- every-day-log
- lambda-calculus
---

### 1 Lambda Calculus

今天和上周日读了两篇lambda-calculus的介绍，一份是SEP的[lambda calculus词条](plato.stanford.edu/entries/lambda-calculus/)，另一份是
Henk Barendregt Erik Barendsen的[Introduction to Lambda Calculus](http://www.cse.chalmers.se/research/group/logic/TypesSS05/Extra/geuvers.pdf)。还有一份讲lambda calculus前世今生的文章，[Lambda Calculus Then and Now](http://turing100.acm.org/lambda_calculus_timeline.pdf)

第一份读了两遍，第二份前几章看的比较认真，后面几张浏览了一下。借了本教材再细看。第一份偏数学，第二份偏背景知识。

Lambda calculus是一种calculus（什么是calculus）。它同图灵机等价(什么是图灵机，什么是等价)。Lambda calculus是functional programming的基础。虽然无法回答括号中的问题，但是我从self-application，或者说recursive中，看到了“计算”的感觉。我也从上面的材料中看到了命题逻辑、数论的影子。

和力学系的同学交流，才知道Lambda calculus和他们学的泛函分析很像（泛函，而非分析）。即使是只看了上面两篇材料，也发觉fixed point combinator是个太巧妙的玩意。用这个技巧来证明简直比数学归纳法还要无赖。

还看到两段话：

> There are also typed versions of the lambda calculus. These are introduced essentially in Curry (1934) (for the so called Combinatory Logic, a variant of the lambda calculus) and in Church (1940).

等于说，Combinatory Logic即typed lambda calculus。

> The two original papers of Curry and Church introducing typed versions of the lambda calculus give rise to two different families of systems. In the typed lambda calculi a` la Curry terms are those of the type-free theory. Each term has a set of possible types. This set may be empty, be a singleton or consist of several (possibly infinitely many) elements. In the systems a` la Church the terms are annotated versions of the type-free terms. Each term has (up to an equivalence relation) a unique type that is usually derivable from the way the term is annotated.

发明了一种记忆法。Curry, curry, 咖喱也，混在一团，type-free。而
Church, church, 教堂，结构严谨，term with a unique type deriving from annotation

### 2 Others

在JC的推荐下，看了edX的FP101x，以haskell为工具教functional programming的。最初接触MOOC还是在Coursera刚上线的时候，那是通过了四门课。之后竟然没有一门课坚持到最后，总是被各种各样的事情掣肘，加上自己定力又不够。

haskell本科时有门划水课上过一点。没学到太多东西，最多知道了map和reduce是什么东西。

今天还看完了[《图灵和ACM图灵奖》](http://book.douban.com/subject/10862190/)。不错的书。我希望能读到高屋建瓴、深入浅出的**学科史**。这本书达不到这个要求，但是作为纪传体的闲书看眼界也不错。比较讨厌的是，人物是按照获奖时间拍的，而人物的成就却不随着这个时间。




