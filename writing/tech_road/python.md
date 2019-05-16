python中一个比较明确的问题但是不知道却会引起很多问题的point是，你所要引用的模块是不是在`sys.path`包含的路径里，其次要注意你所引用的模块确实是一个模块（是否包含`__init__.py`文件）

确认path问题的时候也有两个点需要注意：你把需要的path添加到sys.path里，你的添加确实执行了，好了眼见为时，记得查看`sys.path`去确认就好了，因为它记录的是能够引用的path的最终结果

# python

## 最佳实践

使用pyenv管理python版本
独立的开发环境使用virtualenv:
pyenv virtualenv {python-version} {your-env-name}
pyenv activate
pyenv deactivate

pyenv uninstall {your-env-name} or pyenv virtualenv-delete {your-env-name}  

> Removing the directories in\$(pyenv root)/versions and $(pyenv root)/versions/{version}/envs will delete the virtualenv

### jupyter

设置密码：

```shell
$ jupyter notebook --generate-config
$ jupyter notebook password
```

安装插件：

```shell

# Install Jupyterextension package
pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install — user
# Install configurator and enable configurator
pip install jupyter_nbextensions_configurator
jupyter nbextensions_configurator enable
# Install theme
pip install jupyterthemes

```



### scapy

scapy+xpath数据爬取

xpath：解析网页首选xpath

具体解析方式查看github项目[scrapy_demo](https://github.com/chengcx1019/architecture/tree/startup-1/scrapy_demo/parse)(后续分支可能变更，链接可能失效，可以自行去该仓库查找)

### 基本指令



### Django

更多参考[more about django](./django),django社区的优秀生态和内容

#### 关于静态文件

在DEBUG=False暨开发模式下，Django会提供静态文件，但是在DEBUG=True暨生产环境下，Django不再处理静态资源文件，这时需要交由web容器（nginx等）来处理。

#### 处理log信息

python有一套自己处理log的机制，Django直接使用了python的机制，下面的内容基本来自于[django的logging文档](https://docs.djangoproject.com/en/2.1/topics/logging/)。

首先了解四个组件

- Loggers
  - 日志信息分为五个等级，分别是debug，info，warning，error，critical，根据命名就可以感知到它们的优先级。
  - 每条写入logger的信息成为日志记录，每条日志记录都会有带着自己的日志等级，当一条信息被logger处理的时候，这条信息的日志等级就会和logger的日志等级进行比较，如果超出（大于或等于）logger的等级，就会做进一步处理，否则就会忽略。
  - 需要被进一步处理的日志记录就会被传递给handler。
- Handlers
  - 当你的信息写入logger时，处理器用来定义一种特定的日志行为，例如将日志记录输出到屏幕，文件或者网络接口。
  - handler也有日志等级，和logger一致，只有信息的日志等级大于handler的日志等级才会被处理，否则就被忽略了。
  - 一个logger可以设置多个handler，并且handler可以设置不同的等级
- Filters
  - fliter在日志记录由logger传递给handler时进行额外的处理
  - 可以定义一系列filters形成一个filter链
- Formatters

记录统一使用`logger.log(level=logging.INFO,msg="")`,在其中指定log等级

python的logging模块有多种配置方式，django使用了dictConfig，在django的配置文件中，加入LOGGING的配置，详情见[MyBlog的setting配置的LOGGING](https://github.com/chengcx1019/MyBlog/blob/master/myblog/settings.py)。

### 注意

### Python中的一些语法细节

#### 注意事项

- **永远不要使用小数比较结果来作为两者相等的判断依据**

  可以采取`abs(a-b) < 1e-7`作为二者相等的判断

- 

### String

- 字符串前面添加u,r,b的作用
  - u:表示unicode
  - r:表示非转义的原始字符串，如出现r'\n',不转换为换行而只解析为两个字符
  - b:表示bytes



### 参考

《python学习手册第4版》



## python教学需要有的内容

1. 使用入门：

- pyhton的优缺点，
- python与其他语言的对比，首先可以与matlab对比
- python如何运行程序（python解释器介绍）
- python 开发环境（由命令行，idle，ide（pycharm），notebook）逐步介绍，
- 演示notebook运行

2. 类型

- **python的动态类型介绍**（重要）：

  变量存储在对象中，所有的变量名都是对变量的引用，有很多内容值得讲了，大概有20条小点左右吧

- 数字类型

- 集合类型

  set

- 序列

  - 字符串
  - 列表
  - 元组

- 字典

- 文件

3. 语句和语法

   - 赋值

     python的赋值很有特色

   - 打印

     print

   - if语句，while和for循环

   - 迭代器和解析

     - 可迭代对象，迭代协议
     - 文件迭代，字典迭代
     - 列表解析
     - map
     - **生成器函数**（重点）

4. 函数

   - 定义
   - 作用域
   - 参数
     - 参数传递，默认参数，关键字参数，可变参数
   - 函数的高级特性

5. 模块及模块包，高级模块用法

6. 面向对象编程

   - 类class
     - 类对象和实例对象
     - 类继承，**继承搜索树**（重点）
   - 运算符重载
   - 类的设计模式
     - 多重继承
     - 工厂函数
     - 抽象超类
   - 类的高级主题
   - 类陷阱

7. 异常

8. 装饰器、元类



