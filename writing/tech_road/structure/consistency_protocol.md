一致性协议

一致性模型

2PC

3PC

Paxos

ZAB

## exactly once vs at least once vs at most once

各自定义：

- at most once 最多一次

  消息会被处理0次或者1次，即消息有可能被丢失

- at least once 至少一次

  消息处理会重复但是不会丢失

- exactly once 精确一次

  消息不会丢失也不会重复

从上至下，代价越高，性能越差

基于每种保证的 不同特性，就存在代价和性能的权衡和取舍，那么具体我们该怎么做呢，什么场景下，不可靠的消息传递是我们可以接受的



## reference

[消息传送可靠性](https://doc.akka.io/docs/akka/current/general/message-delivery-reliability.html?language=scala) 