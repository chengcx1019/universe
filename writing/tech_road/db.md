## 数据库join操作

以下面两张表为例理解join操作

|  LastName  | DepartmentID |
| :--------: | :----------: |
|  Rafferty  |      31      |
|   Jones    |      33      |
| Heisenberg |      33      |
|  Robinson  |      34      |
|   Smith    |      34      |
|  Williams  |    `NULL`    |

| DepartmentID | DepartmentName |
| :----------: | :------------: |
|      31      |     Sales      |
|      33      |  Engineering   |
|      34      |    Clerical    |
|      35      |   Marketing    |

- cross join

  [Cartesian product](https://en.wikipedia.org/wiki/Cartesian_product) 

- inner join

  需要根据规则严格匹配

- out join

  - left out join

    以左表数据为基准，没有匹配规则则用null补充

  - right out join

    反之

  - full out join

    聚合两者的结果

  ![image-20190605170837653](/Users/changxin.cheng/Library/Application Support/typora-user-images/image-20190605170837653.png)

