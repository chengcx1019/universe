> pocket 阅读，当前也不确定这种记录方式是否高效，可以带来多大收益，出发点是认为读过的内容即便在 `pocket` 内归档、标注了，但是事后回顾查找仍然很难，记忆中依稀记得关键词如果搜索无果，就束手无策了。如果对印象深刻的文章，把想法思考，整理集中记录起来，这样重新查找到的概率也会高一些，希望在计划执行的过程中能够有更好的方式，目前而言，当前是我能想到的最佳方案了。



## 内容概要列表及简评

***

### 20200329



#### [一致性哈希——一个高效、可扩展的数据分配算法](https://medium.com/@animeshgaitonde/consistent-hashing-an-efficient-scalable-data-distribution-algorithm-a81fc5c0a6c7) 



一致性哈希算法用于解决数据分区问题，目前被 cassandra , Rika, Dynamo DB 等广泛使用。

能够使用达到 `O(1)` 读写的数据结构是 `HashMap` ,但是在分区存储的结构中，普通的 `HashMap` 结构存在一下两个问题：

- `hash(data)%N` 与 `N` 强相关，如果进行横向扩展，需要对之前的所有数据进行重新分配

- 依然与 `N` 相关，如果机器损坏需要被移除，依然要进行数据的重新分配

一致性哈希算法可以解决这类问题，算法绑定的是 server 的 ip 地址，对每一个 server ， 计算 `hash(ip)%360.00` ,同样对于数据，计算 `hash(data)%360.00`  ，即将hash值映射到圆上的一个点，之后按时钟顺序匹配数据最相近的 server ，这样带来的好处是显而易见的，增加或者减少 server 时，只有两个 server 之间的数据或者被减少的 server 上的数据需要重新分配。

同时为了进一步平衡每个 server 的负载，增加虚拟节点的概念，每个 server 都会映射几个虚拟节点，均匀分布在环上。



![tony](https://miro.medium.com/max/1000/1*6W1FxcFdYJkvEm06UOHH4g.gif)



> 从作者原文发现一张图，摘录在此，加深印象



***

### 20200330



#### [一个全职 GO 工作者的工作感受](https://blog.sbstp.ca/go-quirks/) 

总体来看是爱之深，责之切，作者提出了一系列使用过程中潜在的问题，以及不太优雅的解决方案，与之同时作者对 GO 语言开发组广泛听取社区反馈，并将在 GO 2 版本中优化表示赞许

后续如果需要深度使用 GO ， 也许可以有借鉴之处。



#### [一款帮助你专注的app](https://iamintrovert.co/) 

作者放出了[源码](https://github.com/IamIntrovert/Introvert) ，下载 app 看了下，最少等待十分钟，程序会根据一个生成算法生成一幅画，播放30次后，就可以获得一幅画，很遗憾，我第一个十分钟都没有看完，以后有空再看看，没有效果的话我就把这条删了。



#### [异步编程，阻塞 I/O 和非阻塞 I/O](https://luminousmen.com/post/18) 

让我感兴趣的是作者在谈及OS线程对共享数据进行访问时，提到了 GIL ，不过GIL可以在多线程中达到同步作用，而且在 GIL下执行阻塞IO操作也问题不大，作者混淆了一个点就是这本身就是单核单进程下多线程的特性，不管有没有 GIL 锁，这点都可以达到，有了 GIL 反而在多核下，一个进程也只能使用一个核，这是 GIL 的最大弊端。不过兼听则明，这是一个系列博客，整体内容与语言无关，重点是想讲清楚异步这个概念，示例的说明是用    python 写的，看来作者很喜欢python



***

### 20200331



#### [Typing Lessons](https://www.keybr.com/) 

如何提高打字速度的同时减少错误？作者建议是形成像钢琴家一样的肌肉记忆，这个需要长期的练习，但还是那句话，无论从何时开始都不晚，重要是找到一个正确的方式，并立即开始实践。你可能会问，什么是正确的方式，如果你不是很确定，也不是很认同其他人，那就找一种普遍被认可的标准，在操作的过程中，找到适合自己的方式，但不要从一开始就否定所有的可能性。

[Keybr.com](Keybr.com) 就是这样一个帮助你提升打字效率的网站，它有三个要点：一是选择语言中的常用词组进行练习，二是针对你不熟练的组合提供更多的练习，三是根据学习进度，不断扩大所需的键位，提供更大范围的练习。

可以选择英语开始练习，以熟悉键位为主。



***

### 20200401



#### [WAPI RADIO](https://wapi.fm/) 

一个只播放24小时的广播，邀请嘉宾谈论建立人们喜欢的开发者体验，有意思的一点是，只有开始按钮，没有进度条可供拖动。



#### [微服务架构的安全模式](https://developer.okta.com/blog/2020/03/23/microservice-security-patterns) 

安全微服务架构的11种模式：

1. Be Secure by Design：通过设计确保安全
2.  Scan Dependencies：扫描依赖以发现其中潜在的风险
3. Use HTTPS Everywhere：任何使用http的地方都应该替换为[https](https://howhttps.works/) ，免费的证书可以使用*Let’s Encrypt* ，这正是目前我在使用的
4. Use Access and Identity Tokens
5. Encrypt and Protect Secrets
6. Verify Security with Delivery Pipelines
7.  Slow Down Attackers
8.  Use Docker Rootless Mode
9.  Use Time-Based Security
10. Scan Docker and Kubernetes Configuration for Vulnerabilities
11. Know Your Cloud and Cluster Security



> 目前只读到第3条，后面的条目信息量较大，明天碎片时间继续阅读



***

### 20200402



#### [请不要再用 markdown 来写你的文档了](https://buttondown.email/hillelwayne/archive/please-dont-write-your-documentation-in-markdown/) 

这篇可以不用点进去，价值不大，我完全是被这个标题吸引进来的；作者认为 markdown只适合编写短文档，而不适合作为工作工具使用，看下来这篇短文的作者对 markdown的使用还是流于入门，肯定不知道 Typora 的存在， 在这里强推一波 [Typora](https://typora.io/) ，一款简约而不简单，让你专注于写作的 markdown 工具，唯一的缺憾是未集成版本管理功能，对比文件 diff 需要在别的软件中进行，不过 Typora 官方也对此进行了解释，版本管理别的软件有做，typora 无意开发这种重复功能，只想专注于提升用户写作体验本身，事实如此，相对它的优点来说，者点缺憾是完全可以接受的。



#### [12个最好的机器学习音频数据集](https://lionbridge.ai/datasets/12-best-audio-datasets-for-machine-learning/) 



#### [常用的医疗 AI 算法](https://rubikscode.net/2020/03/16/top-ai-algorithms-in-healthcare/) 

最长在医疗领域常用的算法如下：

- 支持向量机
- 神经网络
- 逻辑回归
- 判别分析
- 随机森林
- 线性回归
- 朴素贝叶斯
- K 近邻

目前该领域的问题面临的又要问题是信息安全问题，很多数据由于隐私原因而不能用于训练学习。