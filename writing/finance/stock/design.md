# 构建数据分析平台

我需要一个平台整合市面上可以获得的数据，构建经济数据分析平台，达成如下的目的：

1. 计算股票的Shiller PE
2. 通用数据分析



## 实现方案

django后台提供API，superset可视化展示，vue实现基本界面

### 实体设计
```uml
@startuml
entity company_stock {
公司名称
公司代码
股票代码
上市交易所
股价
年分
季度
月
}

entity inflation_rate{
国家代码
年份
通货膨胀率
}

@enduml
```


```uml
@startuml

(*) --> "物品与服务市场"
"生产要素市场" --> (*)

@enduml
```