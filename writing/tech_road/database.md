## TiDB

- 能否支持跨数据中心的容灾？
- 写入速度是否够快？
- 数据保存下来后，是否方便读取？
- 保存的数据如何修改？如何支持并发的修改？
- 如何原子地修改多条记录？

### Raft

1. Leader 选举
2. 成员变更
3. 日志复制



- [ ] 组件一 将region均匀的散布在集群的所有节点上
- [ ] 组件二 记录region在节点上的分布情况



### 关系模型到key-value模型的映射

行数据编码

```go
Key: tablePrefix{tableID}_recordPrefixSep{rowID}
Value: [col1, col2, col3, col4]
```

唯一索引编码

```go
Key: tablePrefix{tableID}_indexPrefixSep{indexID}_indexedColumnsValue
Value: rowID
```

非唯一索引编码

```go
Key: tablePrefix{tableID}_indexPrefixSep{indexID}_indexedColumnsValue_rowID
Value: null
```