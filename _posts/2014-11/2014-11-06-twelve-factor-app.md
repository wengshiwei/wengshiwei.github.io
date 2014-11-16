---
layout: post
title: Twelve-factor App
category:
- every-day-log
- twelve-factor-app
---

### 1. Twelve-factor App
今天认真地研读了昨天提到的[Twelve-factor App](http://12factor.net/)。画了张图

![Twelve-factor App](/assets/posts/2014_11/12-factor-app.png)

Twelve-factor的重点就是把App从开发到运行涉及到的每个环节都给与明确的“名称”。取名的要点是使每个环节明确、独立。

如果不用这十二个factor来划分，可以用更简单的方法来划分，静态的数据比如codebase和config，动作比如build和release，动态的实体比如process和admin process。不过一时没想太细。

这篇文章很好就是了。我看了三遍。

里面有个不理解的地方，logging里面提到，所有的log应该输出到stdout，然后由专门的logging system(作为一个backing service)来处理log。

引用了了自己的博客上的文章 [Logs Are Streams, Not Files](http://adam.herokuapp.com/past/2011/4/1/logs_are_streams_not_files/)来解释这点。

### 2. Omnigraffle

图是用Omnigraffle画的。虽说是个上手很简单的软件，今天还是更新成最新版6.05，看了个lydia的教程。新版本比旧版本清爽了许多。

