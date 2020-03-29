> pocket 阅读，当前也不确定这种记录方式是否高效，可以带来多大收益，出发点是认为度过的内容即便在 `pocket` 内归档、标注了，但是



## 内容概要列表及简评

***

### 20200329



#### [一致性哈希——一个高效、可扩展的数据分配算法](https://medium.com/@animeshgaitonde/consistent-hashing-an-efficient-scalable-data-distribution-algorithm-a81fc5c0a6c7) 



一致性哈希算法用于解决数据分区问题，目前被 cassandra , Rika, Dynamo DB 等广泛使用。

能够使用达到 `O(1)` 读写的数据结构是 `HashMap` ,但是在分区存储的结构中，普通的 `HashMap` 结构存在一下两个问题：

- 

- 



