what’s the difference between an event stream and a database table? Is a Kafka topic the same as a stream? How can I best leverage all these pieces when I want to put my data in Kafka to use?



## Events, streams, and tables

> 了解Kafka的内部工作原理有助于理解Kafka的行为，也有助于诊断问题



## 基本概念

存储，topics，partitions

设计上的优势：

1. kafka本身不对数据进行序列化和反序列化

2. 将生产者和消费者分离是另一个有利于扩展性的设计

   

**kafka 使用主题来存储数据，每个主题被分为若干个分区，每个分区有多个副本。副本被保存在 broker 上，每个broker 可以保存成百上千个属于不同主题和分区的副本。**





topics里的消息如何记录消费状态，或者说消费的模式是怎样的

brokers：一个独立的 kafka 服务器称为 broker 



kafka集群通过分区对主题进行横向扩展，为主题选定分区数量并不是一件可有可无的事情，需要考虑一下几个因素:

- 主题需要达到多大的吞吐量

需要多少个 broker，搭建集群的考量因素：

- 



scalability, elasticity, and fault tolerance



## zookeeper 在 kafka 中的使用

> kafka 的分布式任务是如何被 zookeeper 协调的

Kafka 使用 Zookeeper保存集群的元数据信息和消费者信息（broker、主题和分区的元数据信息）。

<img src="/Users/changxin.cheng/Library/Application Support/typora-user-images/image-20201208111145183.png" alt="image-20201208111145183" style="zoom:50%;" />

zookeepe r结点个数的选取：zk使用一致性协议，节点过多会降低整个群组的性能

生产者直接连接kafka broker，消费者直接连接的是zk（在后面的版本里消费者也是直接和 kafka 的 broker 相连）



#### 集群成员关系

- 在 broker 启动的时候，它通过**创建临时节点**把自己的 ID 注册到 Zookeeper，Kafka组件订阅 Zookeeper 的 /brokers/ids 路径（broker 在 zk 上的注册路径），当有 broker 加入集群或退出集群时，这些组件就可以获得通知

- 在 broker 停机、出现网络分区或长时间垃圾回收停顿时，broker 会从 zk上断开连接，此时 broker 在启动时创建的临时节点会自动从 Zookeeper 上移除，监听 broker 列表的kafka组件会被告知该 broker 已移除

#### 控制器

- 集群里第一个启动的 broker 通过在 zk 里创建一个临时节点 /controller 让自己成为控制器，其他 broker 在控制器节点上创建 zookeeper watch 对象，接收这个节点的变更通知，以次方式确保集群里一次只有一个控制器存在

- 当 控制器被关闭或者与zk断开连接，临时节点就会消失，其他 broker 收到通知，会尝试让自己成为新的控制器，第一个在zk成功创建控制器节点的 broker 会成为新的控制器，新选出的控制器通过zk的条件递增，获得一个全新的、数值更大的 controller epoch，其他 broker 在知道当前的 epoch 后（<u>如何知道？</u>），如果收到由控制器发出的包含较旧 epoch 的消息，就会忽略它们

- broker 的退出与加入

  - 退出：当控制器发现一个 broker 已经离开集群（通过 watch 相关的 zk 路径），它就知道，那些失去首领的分区需要一个新首领（分区首领刚好在这个 broker 上）；控制器遍历这些分区，并确定谁应该成为新的首领（简单来说就是分区副本列表里的下一个副本），然后向所有包含新首领或现有跟随者的broker 发送请求。该请求消息包含了谁是新首领以及谁是分区跟随者的信息。随后，新首领开始处理来自生产者和消费者的请求，而跟随者开始从新首领那里复制信息
  - 加入：当控制器发现一个broker加入集群，它会使用 broker ID 来检查新加入的 borker 是否包含现有分区的副本。如果有，控制器就把变更通知发送给新加入的broker和其他broker，新broker上的副本开始从首领那里复制消息。

  简而言之， kafka 使用 zk 的临时节点来选举控制器，并在节点加入集群或退出集群时通知控制器。控制器负责在节点加入或离开集群时进行分区首领选举。控制器使用epoch 来避免“脑裂”，“脑裂”是指两个节点同时认为自己是当前的控制器





## 3生产者-向 kafka 写入消息

### 顺序保证

kafka 可以保证同一个分区的消息时有序的。

但是错误的情况有时无法保证，$retries$ 设为非零整数，同时把 $max.in.flght.requests.per.connection$ 设为比 1 大的数，那么，如果第一个批次消息写入失败，而第二个批次写入成功，broker 回重试写入第一个批次。如果此时第一个批次也写入成功，那么两个批次的顺序就反过来了。



一般来说如果某些场景要求消息是有序的，那么消息是否写入成功也很关键，$retries$ 不设为 0，$max.in.flght.requests.per.connection$ 设为 1，这样，生产者尝试发送第一批消息时，就不会有其他的消息发送给 broker。这样做的弊端是会严重影响生产者的吞吐量，所以只有在对消息的顺序有严格要求的情况下才能这么做。

### 序列化与反序列化

schema 注册表，序列化器和反序列化器分别负责处理 schema 的注册和拉取



### 分区

**拥有相同键的消息将会被写到同一个分区**，如果不指定键，那么记录将随机的发送到主题内各个可用的分区上。分区器使用轮询（Round Robin）算法将消息均衡地分布到各个分区上。

#### 配置



## 4消费者

消费者和消费者群组

一个群组里的消费者订阅的是同一个主题，每个消费者接受主题一部分分区消息

？同一个docker服务下的多个 pod 对同一主题是如何消费的，何为不同的消费者

消费群组，同一个消费群组下的消费者订阅某些分区的消息

不同的消费群组，都可以获取到主题下的所有消息



#### 异常情况处理

某个消费者关闭或者崩溃时，他就离开群组，原本由它读取的分区将由群组里的其他消费者来读取，分区的所有权从一个消费者转移到另一个消费者，这样的行为被称为均衡



#### 安全的再均衡



#### 轮询

群组协调，分区再平衡，发送心跳和获取数据



#### 配置

$auto.offset.reset$ :

#### 提交和偏移量



#### 再均衡监听器



## 5深入kafka



#### 复制

复制功能是kafka的核心，在kafka的文档里，kafka 把自己描述为一个分布式的、可分区的、可复制的提交日志服务，复制如此关键的原因是因为它可以在个别节点失效时仍能保证 kafka 的可用性和持久性

**kafka 使用主题来存储数据，每个主题被分为若干个分区，每个分区有多个副本。副本被保存在 broker 上，每个broker 可以保存成百上千个属于不同主题和分区的副本。** 

##### 首领副本和跟随者副本

- 首领副本：每个分区都有一个首领副本。为了保证一致性，所有**生产者的请求和消费者请求**都会经过这个副本。

- 跟随者副本：首领以外的副本都是跟随者副本。跟随者副本不处理来自客户端的请求，他们唯一的任务就是从首领那里复制消息，保持与首领一致的状态。如果首领发生崩溃，其中的一个跟随者会被提升为新首领。
- 首领的另一个任务是搞清楚哪个跟随者的状态与自己是一致的。跟随者为了保持与首领状态一致，在有新消息到达时尝试从首领那里复制消息，不过有各种原因会导致同步失败（网络拥塞导致复制变慢，broker 崩溃导致复制滞后，直到重启 broker 后复制才会继续）
- 首选首领：创建主题时选定的首领就是分区的首选首领，在创建分区时，需要在 broker 之间均衡首领（<u>均衡算法是什么？</u>），这个设计的初衷就是希望首选首领在成为真正的首领时，broker 间的负载最终会得到均衡；kafka 会根据配置进行检查，首选首领是不是当前首领，如果不是，并且该副本是同步的，那么就会触发首领选举，让首选首领成为当前首领
- 首领副本的另一个任务是搞清楚哪个跟随者的状态与自己是一致的。如果一个副本无法与首领保持一致，在首领发生失效的时，它就不可能成为新首领

#### 处理请求

broker 的大部分工作是处理客户端，分区副本和控制器发送给分区首领的请求

- 客户端：生产或者消费
- 分区副本：请求复制
- 控制器发送给分区副本：控制器发现有节点加入或者退出（<u>具体情况还需要进一步了解</u>）

请求的处理方式，broker 会在它所监听的每一个端口上运行一个 Acceptor 线程，这个线程会创建一个连接，并把它交给 Processor 线程去处理（数量可以配置），Processor 线程负责从客户端获取请求消息，把它们放进请求队列，然后从响应队列获取响应消息，把它们发送给客户端

请求类型：

- 生产请求：生产者发送的请求，它包含客户端要写入 broker 的消息
  - 生产者用 acks 参数指定需要多少个 broker 确认才可以认为一个消息写入是成功的，如果acks=1，那么只需要首领收到消息就认为写入成功，同理，如果 acks = all ，需要所有同步副本收到消息才算写入成功（<u>如何判断所有副本都收到消息？</u>），如果acks=0，那么生产者在把消息发出去后，完全不需要等待 broker 的响应
    - 副本首领如何判断所有副本都收到消息：请求被保存在一个叫作“炼狱”的缓冲区里，直到副本首领都复制了消息，响应才会被返回给客户端（<u>是同步的等待过程吗？即 kafka 本身不对响应时间做任何保证</u>）
  - 包含首领副本的 broker 在收到生产请求时，会对请求做一些验证：
    - 发送数据的用户是否有主题的写入权限
    - acks 的值是否有效（只允许出现 0、1 或 all ）
    - acks=all 时是否有足够多的同步副本保证消息已经被安全写入
  - 数据本地化：消息会被写到文件系统缓存里，并不保证何时被刷新到磁盘上（这是操作系统文件管理本身的性质和保证），kafka 不会一直等待数据被写到磁盘上——它依赖复制功能来保证消息的持久性
- 获取请求：在消费者和跟随者副本需要从 broker 读取消息时发送的请求
  - 客户端发送请求，向 broker 请求主题分区里具有特定偏移量的消息
  - 客户端还可以指定 broker 可以从一个分区里返回多少数据
  - 首领副本在收到请求时，会检查请求是否有效：
    - 指定的偏移在分区是否存在
  - kafka使用<u>零复制技术？</u>向客户端发送消息
  - 客户端也可以设置从 broker 返回数据的下限，这样可以在消息流量不是很大的情况下，减少来回通信的次数;与之同时，客户端也不可能一直等待，客户端可以定义一个超时时间，告诉 broker ，如果不能在x时间内累积数据量，那么就把档期哪这些数据返回给我
  - 并不是所有保存在分区首领上的数据都可以被客户端读取，在消息还没有被写入所有的同步副本（<u>所有同步副本的定义是什么？</u>）之前，是不会发送给消费者的（尝试获取这些消息的请求会得到空的响应而不是错误）。这是因为没有被足够多副本复制的消息被认为是不安全的（如果首领发生崩溃，另一个副本成为首领，那么这些消息就丢失了，如果允许消费者读取这些消息，就会破坏一致性）

<img src="/Users/changxin.cheng/Library/Application Support/typora-user-images/image-20201210142849412.png" alt="image-20201210142849412" style="zoom:50%;" />

消费者只能看到“同步完”的消息：

<img src="/Users/changxin.cheng/Library/Application Support/typora-user-images/image-20201210145038991.png" alt="image-20201210145038991" style="zoom:50%;" />

注意事项：

- 生产请求和获取请求都必须发送给分区的首领副本。kafka 客户端要自己负责把生产请求和获取请求发送到正确的 broker 上

  - （<u>如何保证</u>）：客户端使用元数据请求，客户端指定感兴趣的主题列表，服务器响应消息里指明了这些主题所包含的分区、每个分区都有哪些副本，以及哪个副本是首领。元数据请求可以发送给任意一个 broker，因为所有 broker 都缓存了这些信息
  - 客户端元数据缓存：客户端会缓存这些信息，并且需要定时发送元数据请求来刷新这些信息；另外，客户端收到“非首领”错误后，会在重发请求之前先刷新元数据

   

#### 物理存储

kafka 的基本存储单元是分区

- 首先，数据是如何被分配到集群的 broker 上以及 broker 的目录里的
- 然后，broker 是如何管理这些文件的，特别是如何进行数据保留的
- 随后，深入探讨文件和索引格式
- 最后，讨论日志压缩及其工作原理

##### 分区分配

基于一个实例进行分析：6个 broker ，创建一个包含10个分区的主题，并且复制系数为3，那么会有30个分区副本，他们可以被分配给6个broker，在进行分区分配时，需要达成以下目标：

-  在 broker 间平均的分布分区副本
- 确保每个分区的每个副本分布在不同的broker上
- 如果为 broker 指定了机架信息，尽可能把每个分区的副本分配到不同机架的 broker 上



## 6可靠的数据传递

- 各种各样的可靠性及其在kafka场景中的含义
- kafka 复制功能，以及它是如何提高系统可用性的
- 如何配置kafka 和broker 和主题来满足不同的使用场景需求
- 如何验证系统的可靠性

### 可靠性保证

- 分区消息的顺序性保证
- <u>只有当消息被写入分区的所有同步副本时</u>，它才被认为是“已提交”的；生产者可以选择接受不同类型的确认（这两者其实是解耦的，那如果生产者指定的不是acks=all，那么就存在生产者以为成功但是写失败的情况）
- 只要还有一个副本是活跃的，那么“已经提交”的消息就不会丢失
- 消费者只能读取已经提交的消息

构建一个可靠的系统需要作出一些权衡，这种权衡一般是指消息存储的可靠性和一致性的重要程度与可用性、高吞吐量、低延迟和硬件成本的重要度之间的权衡

### 复制和分区可用性

复制机制和分区的多副本架构是kafka可靠性保证的核心。把消息写入多个副本可以使kafka发生崩溃时仍能保证消息的持久性





## 事务



## 同类产品对比

RocketMQ

特点：解耦、削峰填谷（在面临流量的不确定性时，实现对流量的缓冲处理）

存储形式

存储可靠性

顺序消息

延时消息

消息重复

消息过滤

消息失败重试

DLQ（dead letter queue）

回溯消费

事务

 

服务发现

高可用

