---
title: 如何获得高并发的经验？
shortTitle: 如何获得高并发的经验？
description: 现实中，哪怕是大公司，高并发系统也是可遇不可求的。不过，高并发其实是可以通过压测来模拟的。高并发的…
tags:
  - 互联网
  - 程序员
  - 编程
  - .NET
  - Java 程序员
author: 阿里巴巴大淘宝技术 
category:
  - 知乎
head:
  - - meta
    - name: description
      content: 现实中，哪怕是大公司，高并发系统也是可遇不可求的。不过，高并发其实是可以通过压测来模拟的。高并发的…
  - - meta
    - name: keywords
      content: 互联网,程序员,编程,.NET,Java 程序员
---

**现实中，哪怕是大公司，高并发系统也是可遇不可求的。不过，高并发其实是可以通过压测来模拟的。**

高并发的背后，核心是高可用和低延迟。所以我们其实是想有能力设计一个系统，在高并发访问的时候，系统依然可用，而且响应速度不会变慢。

  

**（点击头像关注我们账号，别错过更多阿里工程师一线技术干货）**

————————————————————————————————————————  

想提升高并发系统的设计和开发能力，有2个方面：

**一个是系统的学习相关理论；**

**一个是找一个目标系统，不断想办法去提升他的性能。**

前者是后者的理论基础。

如果想从事一个高并发系统开发的岗位，要学习的相关技术其实是很多的，这些技术核心就是解决高并发情况下如何保持系统的高可用和低延迟。

以Java工程师为例，互联网程序员面试中经常会考察的内容包括：

**（1） 架构设计：**

高可用与稳定性、事务一致性、多副本一致性、CAP理论。

**（2） 相关技术：**

多线程（JUC/AQS/线程池）、RPC调用及框架（如Thrift）、NIO及NIO框架（如Netty）、高并发框架（如Disruptor） 、微服务框架（SpringBoot）、微服务治理（Spring Cloud）、数据库相关技术（如：索引优化、分库分表、读写分离）、分布式缓存（如redis）、消息中间件系统（如RabbitMQ）、容器技术（如docker）。

**（3） 工具：**

系统性能查看（top、uptime、vmstat、iostat）、压测工具（如ab、locust、Jmeter、go）、线程分析（如jps、jstack）等。

当然，一开始，我们不可能逐一把这些技能全部掌握，我们可以从一个实际项目入手，不断的把这些技术用上去，发现哪些知识不足，再去补充相关的知识。

**“如何设计一个好的秒杀系统“，一定是互联网大厂面试中最常问的一个问题。所以从设计一个秒杀系统开始实践，是个不错的选择。**

秒杀系统的特点：
--------

**（1）瞬时并发量大**

秒杀时会有大量用户在同一时间进行抢购，瞬时并发访问量突增 10 倍，甚至 100 倍以上都有。

**（2）库存量少**

一般秒杀活动商品量很少，这就导致了只有极少量用户能成功购买到。

**（3）业务简单**

流程比较简单，一般都是下订单、扣库存、支付订单。

设计秒杀系统的关键点：
-----------

**（1）限流**

由于活动库存量一般都是很少，对应的只有少部分用户才能秒杀成功。所以我们需要限制大部分用户流量，只准少量用户流量进入后端服务器。

**（2）削峰**

秒杀开始的那一瞬间，会有大量用户冲击进来，所以在开始时候会有一个瞬间流量峰值。如何把瞬间的流量峰值变得更平缓，是能否成功设计好秒杀系统的关键因素。实现流量削峰填谷，一般的采用缓存和 MQ 中间件来解决。

**（3）异步**

秒杀其实可以当做高并发系统来处理，在这个时候，可以考虑从业务上做兼容，将同步的业务，设计成异步处理的任务，提高网站的整体可用性。

**（4）缓存**

秒杀系统的瓶颈主要体现在下订单、扣减库存流程中。在这些流程中主要用到 OLTP 的数据库，类似 MySQL、Oracle。由于数据库底层采用 B+ 树的储存结构，对应我们随机写入与读取的效率，相对较低。如果我们把部分业务逻辑迁移到内存的缓存或者 Redis 中，会极大的提高并发效率。

从0到1搭建一个秒杀系统，也并不容易，涉及到很多前端、后端、中间件的技术。这个跟其实是所有公司的工作常态，大部分时间也是在搭架子，真正做技术优化的时间并不多，经常是在业务量突增或者大促活动来临时，集中搞一波性能优化。

所以，如果没有实际的高并发项目可做，自己弄个秒杀系统自娱自乐也是不错的。

**搭建系统 -> 压测 -> 发现问题 -> 学习知识 -> 优化系统，通过这样的循环，相信你一定既能体验到学习的乐趣，同时实力也大幅提升。**

（淘系技术部-产品技术-昭明）

  

————————————————————————————————————————

阿里巴巴集团淘系技术部官方账号。淘系技术部是阿里巴巴新零售技术的王牌军，支撑淘宝、天猫核心电商以及淘宝直播、闲鱼、躺平、阿里汽车、阿里房产等创新业务，服务9亿用户，赋能各行业1000万商家。我们打造了全球领先的线上新零售技术平台，并作为核心技术团队保障了11次双十一购物狂欢节的成功。详情可查看我们官网：[阿里巴巴淘系技术部官方网站](https://link.zhihu.com/?target=http%3A//tech.taobao.org)

点击下方主页关注我们，你将收获更多来自阿里一线工程师的技术实战技巧&成长经历心得。另，不定期更新最新岗位招聘信息和简历内推通道，欢迎各位以最短路径加入我们。

[](https://www.zhihu.com/org/a-li-ba-ba-tao-xi-ji-zhu)

转载链接：[https://www.zhihu.com/question/40609661/answer/1883655079](https://www.zhihu.com/question/40609661/answer/1883655079)