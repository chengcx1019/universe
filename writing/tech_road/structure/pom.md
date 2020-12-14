

pom 组织原则

- dependencies 和 dependencyManagement

  dependencyManagement 用在有继承关系的模块中，比如 parent 和 sub-1，sub-2，parent 在 pom 文件中的 <dependencyManagement> 中定义所需要的依赖及对应版本，子模块将需要使用的模块定义在自己 pom 中的 <dependencies> 下,这时可以不配置版本，使用的<version>即是在<dependencyManagement>节点中配置的相应<version> ，这样可以使得多个子模块中引入相同依赖的版本号保持一致

- 在同一个 pom 文件中，<dependencyManagement> 和 <dependencies> 的关系是什么

  以 <dependencyManagement> 为准，同时如果在 <dependencies> 中定义了的话，则认为子模块一定会引入该依赖，且是以 <dependencyManagement>中的版本为准，这样的效果和在<dependencies>中引入该依赖并且指定版本效果一致

- 引入某个 pom，会引入关联依赖吗

  会自动引入依赖的依赖，但也有一些限制，哪些依赖不会被引入

  - *Dependency mediation*：当有多个版本被引入时，选取的策略是在依赖树中层级更小的依赖会被选中（即引用层次更少的会被引入）

    如果试图打破这个规则，就在 pom 文件中指定需要的依赖及版本，打破这个规则的方式其实本身是遵循了这个规则（即为了指定某个依赖的版本，其实是引入了一个模块并不直接使用的依赖）

    ```
      A
      ├── B
      │   └── C
      │       └── D 2.0
      └── E
          └── D 1.0
    ```

    ```
    A
      ├── B
      │   └── C
      │       └── D 2.0
      ├── E
      │   └── D 1.0
      │
      └── D 2.0
    ```

    当前模块 Dependency management 内的模块是不会直接引用的，所管理里，<u>仅仅是在出现传递依赖时指定默认版本？（这个机制不是得在父子模块发生继承时才生效吗）</u>

  - Dependency management* 

    参考前两条

  - *Dependency scope*

    在 build 的不同阶段正确的引入依赖

  - *Excluded dependencies* 

    排除依赖

  - *Optional dependencies*

  - 最终只会使用某一 server 版本



### 