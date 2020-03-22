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
时间
open
high
low
close
市值
流通股份
利润
每股收益
资产（账面净资产）
}

entity company_detail{

}

entity inflation_rate{
国家代码
年份
通货膨胀率
}

@enduml
```

数据存储模式很重要，如何兼容多种数据导入模式
支持灵活定义的数据存储方式
每天更新数据，数据源，数据爬取
定时任务管理
https://github.com/PanJiaChen/vue-element-admin
https://github.com/Jack-Cherish/python-spider