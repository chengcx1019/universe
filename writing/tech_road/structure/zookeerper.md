# zookeeper 在分布式系统中的应用



> 分布式数据一致性解决方案

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



### broadcast

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



### 监听器

1. **客户端向 ZooKeeper 服务器注册一个 Watcher 监听；**
2. **把这个监听信息存储到客户端的 WatchManager 中；**
3. **当 ZooKeeper 中的节点发生变化时，会通知客户端，客户端会调用相应 Watcher 对象中的回调方法。** 



<u>server 端如何实现 watch 注册机制，如何管理对节点的监听，是否可以保证一致性（即确保 client 一定会收到节点变更的通知）</u> 





两个问题：

1. 如何注册 watcher

2. 客户端回调



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

