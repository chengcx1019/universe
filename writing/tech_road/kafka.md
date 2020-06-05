what’s the difference between an event stream and a database table? Is a Kafka topic the same as a stream? How can I best leverage all these pieces when I want to put my data in Kafka to use?



## Events, streams, and tables





part2

存储

topics

partitions



topics里的消息如何记录消费状态，或者说消费的模式是怎样的

brokers

设计上的优势：

1. kafka本身不对数据进行序列化和反序列化
2. 将生产者和消费者分离是另一个有利于扩展性的设计





scalability, elasticity, and fault tolerance

