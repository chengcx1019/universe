- 分布式一致性协议


完整的**Libra protocol**包含以下部分：
- logical data model：分布式数据库
- exciting transaction：move
- authenticated data structures and storage：Merkle tree
- Bzantine Tault Tolerant Consensus
- Networking：验证节点间的交互

## 2 Logical Data Model

### Ledger State

Move resources store data values and module store code

module/struct/procedure类比面向对象class/object/method

module无外部field，且只有静态方法

resource由module申明，resource的类型由申明module的类型和地址组合定义，例如`0x56.Currency.T`，module的名称是Currency，存储的地址是0x56,resource的名称是`currency.T`,

resource的类型是`0x56.Currency.T`



### Transaction

客户端通过提交交易来改变账号状态

## 3  exciting transaction

### Execution Requirements

- Known initial state

  

- Deterministic

  状态不可变，可从原始状态执行所有交易历史达到当前状态

- Metered

  为交易收取费用

- Asset semantics

  保证数字货币不能丢失 ，重复，或者无授权交易

### TransactionStructure

- Sender address

- Sender public key

- Program

- Gas price

- Maximum gas amount

- Sequence number

  

### ExecutingTransactions

1. Check signature
2. Run prologue
   - 校验公钥
   - 检查账户余额
   - 校验交易版本，反之旧的交易反复执行
3. Verify transaction script and modules
4. Publish modules
5. Run transaction script
6. Run epilogue
   - 收费
   - 交易成功，增加sender账号下的序列号

### The Move Programming Language

- Transaction scripts
- Modules
- The Move virtual machine



## 4 Authenticated Data Structuresand Storage

1. Backgroundon Authenticated Data Structures

   Merkle Tree

2. Ledger History

3. Ledger State

4. Accounts

5. Events

   3，4，5亟需进一步理解

## 5 Byzantine Faul tTolerant Consensus

HotStuff

1. Overview of LibraBFT
2. Advantages of the HotStuff paradigm
3. HotStuff extensions and modifications
4. Validator management

## 6 Networking

## 7 Libra Core Implementation

**libra core包含以下模块**：

- Admission control
- Mempool
- Consensus
- Transaction execution
- 

### 7.1 Write Request Lifecycle

1. Admission control

2. Mempool

3. Consensus

4. Transaction execution

5. Committing a block

   本地存储方式

### 7.2 ReadRequestLifecycle

## 8 Performance

1. Throughput: The number of transactions that the blockchain can process per second.

2. Latency: The time between a client submitting a transaction to the blockchain and another

   party seeing that the transaction was committed.

3. Capacity: The ability of the blockchain to store a large number of accounts.



- Protocol design
- Validator selection

## 9 Implementing Libra Ecosystem Policies with Move

### 9.1 LibraCoin

### 9.2 Validator Management and Governance

### 9.3 ValidatorSecurityandIncentives

## 10 What's Next for Libra?



一个满足Libra交易条件的客户端需要哪些前置条件

本地存储是什么实现的

不同的validator之间是如何验证数据准确性的

共识协议，以及对某个区块达成共识后，各状态如何同步，同步给其他节点失败时怎么办