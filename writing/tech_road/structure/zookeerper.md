# zookeeper 在分布式系统中的应用



> 分布式数据一致性解决方案，以下的描述中，节点指 zookeeper 集群的某个服务节点，而数据节点统一用 zookeeper 的内部术语 znode 表示

### Zookeeper Atomic Broadcast protocol (ZAB) 

为了提高 server 的稳定性，zk 引入server 集群，通过选择算法选取 leader ，其他集群节点自动成为 follower，leader 通过两阶段提交的方式对其他server发起广播

#### 分布式一致性算法

各自解决了什么问题，缺点是什么

- 两阶段提交

- 三阶段提交

- Paxos

- ZAB

  支持奔溃恢复的原子广播协议， zookeeper 基于此实现了一种主备模式的系统架构来保持集群中各副本之间数据一致性

  - leader 向 follower 提出提案（proposal）
  - leader 需要在达到法定数量（半数以上）的 followers 确认之后才会进行 commit

  ZAB 中的三个角色，leader，follewer，observer：

  - leader ：集群中唯一的写请求处理者
  - follower: 能够响应客户端的的请求，如果是读请求可以自己处理，如果是写请求则要转发给 leader；在选举过程中可以参与投票，有选举权和被选举权
  - observer

  - [ ] 消息广播模式里 leader 发出 proposal 后，收到半数的 ack 会发出 commit，那未发出 ack 的 follower 会如何处理这次 commit 呢

   崩溃恢复模式

- 确保在leader服务器上提交的事务最终被所有的服务器提交



### zookeeper apache 相关生态

curator recipes framework client 之间的关系：

- **curator-framework：**对zookeeper的底层api的一些封装
- **curator-client：**提供一些客户端的操作，例如重试策略等
- **curator-recipes：**封装了一些高级特性，如：Cache事件监听、选举、分布式锁、分布式计数器、分布式Barrier等
  - Cache是Curator中对事件监听的包装，可以看作是对事件监听的本地缓存视图，能够自动为开发者处理反复注册监听

版本兼容性：



Curator 5.0 支持zookeeper3.6.X，不再支持 zookeeper3.4.X

Curator 4.X 支持zookeeper3.5.X，软兼容3.4.X

Curator 2.X 支持zookeeper3.4.X



### broadcast

zookeeper 提供的通知客户端变更的机制为 watches

zk 客户端会在指定节点上注册一个 watcher ，Znode 上节点数据被更新时，服务端就会通知客户端，这个过程通过三部曲来实现：

1. 客户端注册 watcher

   客户端通过 getData、getChildren 和 exist 方法向服务端注册 Watcher
   
   客户端发送 watcher 到服务端注册成功后，会把信息保存到 zk-watch-manager
 ```uml
@startuml
(*)-->"客户端 API 传入 watcher 对象"
-->"标记 request \n 封装 watcher 到 watcher-registration"
-->"向服务端发送请求"
-->[服务端响应成功]"将 watcher 注册到客户端 zk-watch-manager"
-->(*)
@enduml
 ```

2. 服务端处理 watcher

   对于客户端的注册，服务端维护了哪些信息

   服务端收到客户端的请求以后，交给 FinalRequestProcessor 处理，这个进程会去 ZNode 中获取对应的数据，同时会把 watcher 加入到 watch-manager 中

   这样下次节点上的数据更改了以后，就会通知注册 watcher 的客户端

3. 客户端回调 watcher

   服务端在响应客户端注册后，会发送 watcher-event 事件，作为客户端有对应的回调函数接受这个消息

   

客户端何时发起注册，何时缓存:

启动数据监听时，获取节点下的所有数据，注册并缓存



### Watches —— 监听器

1. **客户端向 ZooKeeper 服务器注册一个 Watcher 监听；**
2. **把这个监听信息存储到客户端的 WatchManager 中；**
3. **当 ZooKeeper 中的节点发生变化时，会通知客户端，客户端会调用相应 Watcher 对象中的回调方法。** 



<u>server 端如何实现 watch 注册机制，如何管理对节点的监听，是否可以保证一致性（即确保 client 一定会收到节点变更的通知）</u> 



#### zookeer watches 机制：

- one-time trigger

  使用 getData 获取了数据后，同时会设置一次watch，只会接收一次变更，后续变更的获取需要注册新的 watch

- sent to the client

  对于设置了 watch 的 change，客户端只会通过watch event 先收到这个变更，<u>倘若这个由 server 发向 client 的 watch event 延迟了，client 在此期间发起了查询会出现什么现象，这个保证还成立吗</u> 

- The data for which the watch was set

  data watches and child watches.

  

watches 提供的保证：

论述的核心都是顺序性，zookeeper 的顺序性保证（强调的是集群内的节点的顺讯性）

- 读顺序

- 写顺序

  zk 保证写入在每个节点的写入顺序是一致的，但是并不保证每个节点会同步更新写入

  <u>既然各个节点的写入是时间上是不一致，如何保证从集群的各个节点读取的数据是一致的呢，还是说zookeeper原本就没有做这个保证？</u>，zookeeper 本身对这个现象有一个专门术语进行描述,"hidden channel",client 可能会读取到“stale data”，解决这个问题的建议方式是客户端直接对某个znode设置监听，不要通过第三方通知再 getData 的方式，案例的描述如下图所示

![image-20201118175739993](/Users/changxin.cheng/Library/Application Support/typora-user-images/image-20201118175739993.png)  

- 通知的顺序

  如果一次更新 b 发生在某个被 watch 的znode（记为 A ）后，client 一定会先收到 A 变更的事件通知，而后才能看到 b 的更新

  对于 zookeeper 书中对配置同步处理的理解：

  发生更新前，master 创建 /config/invalid ，client 需要注册  /config/invalid 的 state watch，只要这个 znode 还存在，就不能读取 /config下其他配置，这时候再发生 /config 下其他配置的更改，配置更改完成以后，删除 /config/invalid ，此时可以开始读其他配置，<u>但是这个方式和描述的这个性质有什么关系，删除 /config/invalid 的操作本身是发生在其他配置更新之后的，保证不会提前判断的方式是客户端需要判断这个节点是否存在这个约束</u> 



#### Watches 的性能问题

大量的 client 监听同一个 znode，一旦该 znode 发生变更，会造成节点响应其他事件发生延迟

一个 watch 会在 server 的 watch manager 增加 250-300 bytes，创建大量的 watch 会极大的消耗服务器内存



#### 使用 zookeeper 时需要注意的潜在的失败情况

 watch 的同步是由客户端触发的



<u>某一节点挂掉，原client注册的监听是否会同步到其他节点</u> 

- 事件的分发都是有序的
- client 如果监听了某个 znode，接受到watch event 会在看到节点新数据之前
- watch event 事件的顺序 和 zk server 上的更新顺序一致

watches 的注意事项：

- watch 是一次性触发器；如果接收了一次 watch event 后，想要继续接受到未来变更的通知，需要设置另一个 watch(在event的回调里设置另一个watch)
- 在收到 event 和 发出新的请求间存在延迟（在你收到 event 并设置新的 watch 之间，znode 可能已经变更了好几次）



周期性 polling 与监听相结合



getData、getChildren、exists 均有对 znode 设置 watch 的选项

一个watch event 包含以下信息：

```java
the state of the zookeeper session
event type: NodeCreated/NodeDeleted/NodeDataChanged/NodeChildrenChanged/None
znode path
```

三者关联:

当event type 为None时，表示 session 状态变更

zookeeper 使用同一套机制来通知session和event的变化



三个问题：

1. 如何注册 watcher

2. 客户端回调

3. 客户端/服务端异常 watches 是否会恢复

   - 客户端服务挂掉后会重新注册，客户端client挂掉后又该如何

     客户端挂掉，无法接收注册的监听，关于这一次丢失该如何

     <u>server 如何管理 client ，以临时节点的方式？</u>

   - 服务器端挂掉会保留watches的信息吗

#### 实现示例

各类监听器的使用及区别

##### PathChildrenCache

特点：

- 永久监听指定节点下的节点
- 只能监听指定节点下一级节点的变化
- 可以监听的事件：节点的创建、节点数据的变化、节点删除

使用方式：

- 创建 `CuratorFramework`的 `client`

- 添加 `PathChildrenCache` 

- 启动 `client` 和 `PathChildrenCache` 

- 注册监听器

  

```java
 CuratorFramework client = CuratorFrameworkFactory.newClient(properties.getZkAddress(), new ExponentialBackoffRetry(1000, 3));

PathChildrenCache cache = new PathChildrenCache(client, testPath, true);
PathChildrenCacheListener listener = new PathChildrenCacheListener() {
  public void childEvent(CuratorFramework client, PathChildrenCacheEvent event) throws Exception {
    log.info("start node watcher,{}", Utility.GSON.toJson(event));
    switch (event.getType()) {
      case CHILD_ADDED: {
        System.out.println("Node added: " + ZKPaths.getNodeFromPath(event.getData().getPath()));
        break;
      }

      case CHILD_UPDATED: {
        System.out.println("Node changed: " + ZKPaths.getNodeFromPath(event.getData().getPath()));
        break;
      }

      case CHILD_REMOVED: {
        System.out.println("Node removed: " + ZKPaths.getNodeFromPath(event.getData().getPath()));
        break;
      }
      default:
        break;
    }
  }
};
cache.getListenable().addListener(listener);
cache.start(PathChildrenCache.StartMode.BUILD_INITIAL_CACHE);

```







Zookeeper 采用 ACL（Access Control Lists）策略来进行权限控制，类似于 UNIX 文件系统的权限控制。Zookeeper 定义了如下 5 种权限：

- **CREATE**: 创建子节点的权限
- **READ**: 获取节点数据和子节点列表的权限
- **WRITE**: 更新节点数据的权限
- **DELETE**: 删除子节点的权限
- **ADMIN**: 设置节点ACL的权限

其中尤其需要注意的是，CREATE 和 DELETE 这两种权限都是针对子节点的权限控制。



### 存储

znode，作为存储媒介，ZNode 分为持久节点和临时节点

- 持久节点（**PERSISTENT**）：该数据节点被创建后，就一直存在于 zookeeper 服务器上，除非删除操作（delete）清除该节点
- 临时节点**（EPHEMERAL）**：该数据节点的生命周期会和客户端会话绑定在一起，如果客户端会话丢失了，那么节点就自动清除掉

节点还有一个属性是 **SEQUENTIAL** 

版本控制

znode 存储大小限制



zookeeper 的数据存储在内存中

[The znode hierarchy is stored in memory within each of the ZooKeeper servers](http://www.corejavaguru.com/bigdata/zookeeper/how-zookeeper-works#:~:text=Each%20ZooKeeper%20server%20also%20maintains,a%20znode%20is%201%20MB.) 



如何在同一 SDK 中，支持客户端连接不同的server，以及其他的组件使用不同版本的客户端；看起来使用一个 client facroty 可以解决问题（如何确定应该使用哪一个client，同时引入多个client会不会存在依赖冲突）

大量的监听与冗余监听哪种方式可以获得更大的收益





### 简介

#### zookeeper的使命

1. zk 改变了什么



- zookeeper 是如何协调任务的，zookeeper可以做什么，不可以做什么
- 作为一个集群，zk 是如何保持自己的一致性和容错性的zab

- 同步原语
- 脑裂问题

- zookeeper 客户端api：
  - 保障强一致性、有序性和持久性
  - 实现通用的同步原语的能力

