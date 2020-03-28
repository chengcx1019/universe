> 计算机体系结构及系统设计

## 计算机体系结构



### 故障排查：顺便考察linux指令

无法访问某网址

###  python语法

#### 模块结构

##### Abstract Base Classs

 `@abstractmethod `和`abstractproperty`decorator 只能在 metaclass 是(或者继承) `ABCMeta`的class中使用。

register(subclass),ABC不会出现在subclass的MRO (Method Resolution Order)中，但issubclass()是有效的

Mix-in class

duck-typing：

一种编程风格，不指定具体类型，允许多种形式的替代来提高灵活性，避免使用type()和isinstance()验证，而更多使用hasattr()及EAFP(Easier to ask for forgiveness than permission)编程。

EAFP:

一种通用编程风格，假定属性存在并直接使用，使用try，except来处理不存在的假设。

##### FAQ



##### parameters和arguments的区别

`parameters`是在函数定义时定义的，而`arguments`是在函数调用时传递给函数的值，parameters定义了函数可以接收arguments的类型。

##### 可变和不可变

操作一个可变对象(list, dict, set, etc)，所有指向对象的变量同时改变，而操作一个不可变对象(str, int, tuple, etc)，会产生一个新的对象，同时变量指向新的对象：

```python
>>>x = list() # list是可变对象
>>>y = x
>>>x.append(10)
>>>x
[10]
>>>y
[10]
>>>x = 5 # int是不可变对象
>>>y = x
>>>x = x + 1
>>>y
5
>>>x
6
```

使用is和id()来判断变量是否引用同一对象。

##### 复制一个对象

`copy()`或`deepcopy()`，序列的切片操作返回的是一个新的对象。

##### 查看对象的方法和属性

`dir(object)`。

##### '?:'操作符

`[on_true] if [expression] else [on_false]`

##### decorator装饰器

一个函数返回另一个函数。

```python
def f(...):
    ...
f = staticmethod(f)

@staticmethod
def f(...):
    ...
```

对某个函数使用decorator时保留该函数重要的元信息

##### descriptor描述器

定义了 __get__(), __set__(), or __delete__()中任意一个的对象。



### 

python语法核心

进程锁

dict底层实现

### 操作系统

#### 基础

- 通道作为一个独立于CPU的处理器，负责数据共享和传输处理的工作单元

  - 通道控制设备控制器，设备控制器控制设备

- 按记录的逻辑结构分，文件分为**堆文件、索引文件、索引顺序和顺序文件**

- MS-DOS 系统中的磁盘文件物理结构属于**链接文件**

- 批处理系统主要指**多道批处理系统**，由于多道程序能交替使用CPU，提高了CPU及其他系统资源的利用率，同时也提高了系统的效率。多道批处理系统的缺点是延长了作业的周转时间，用户不能进行直接干预，缺少交互性，不利于程序的开发与调试。

- 带宽：又叫频宽，是指在固定的的时间可传输的资料数量，亦即在传输管道中可以传递数据的能力。

- 位宽 ：位宽就是内存或显存一次能传输的数据量。

- **函数声明但函数未定义，编译可通过，链接不可通过**

- 孤儿进程将被init进程(进程号为1)所收养，并由init进程对它们完成状态收集工作。

  僵尸进程将会导致资源浪费，而孤儿则不会。

#### 存储

- 随机存取方式是指CPU可以对存储器的任一存储单元中的内容随机存取，而且存取时间与存储单元的物理位置无关
  - CDROM即光盘，采用串行存取方式
  - EPROM，DRAM，SRAM采用随机存取方式

#### 共享与互斥

- 解决死锁的方法
  - 银行家算法：避免死锁
  - 资源有序分配法：预防死锁
  - 资源分配图化简法：检测死锁
  - 撤销进程法：解决死锁

#### 内存管理

- 在分页式、段式和段页式管理中，运行开销可能最大的是分页式管理，分页式存储管理可能将连续的指令放置在不同的页中，会发生换页式中断

- 内存管理方案

  - 覆盖技术：把程序划分为若干个功能上相对独立的程序段，按照其自身的逻辑结构使那些不会同时运行的程序段共享同一块内存区域。程序段先保存在磁盘上，当有关程序的前一部分执行结束后，把后续程序段调入内存，覆盖前面的程序段。 
  - 交换技术：在分时系统中，用户的进程比内存能容纳的数量更多，系统将那些不再运行的进程或某一部分调出内存，暂时放在外存上的一个后备存储区，通常称为交换区，当需要运行这些进程时，再将它们装入内存。
    - 

- 内存管理方式

  - 简单存储管理
    - 可变分区
      - 分配算法
        - 最佳适应：每次分配最小的内存，这种方法能使碎片尽量小；
        - 最坏适应：每次分配最大的内存；
        - 首次适应：地址由低到高，找出一个能满足要求的空闲分区给所需要的请求；
        - 循环首次适应：首次适应的变种，分配内存时，从上次找到的空闲区的下一个空闲处开始查找，该算法可以使得内存中空闲区域分布的较均匀。
  - 分页式存储管理：将程序逻辑地址空间划分为固定大小的页(page)，而物理内存划分为同样大小的页框(page frame)。为方便地址转换，页面大小应是2的整数幂。每一个作业有一个页表，用来记录各个页在内存中所对应的块（页框）。
    - 地址组成：页号+页内偏移
    - 快表，又称联想寄存器(TLB)，用来存放当前访问的若干页表项，以加速地址变换的过程。
  - 请求式分页存储管理，在页式存储管理的基础上，增加了请求调页功能和页面置换功能
    - 页面置换算法
      - 最佳算法（OPT算法）：用来评价其他算法，使用缺页中断率：`f = F / A`（其中F为作业失败访问的次数，A为作业总的访问次数）
      - 先进先出算法（FIFO算法）：淘汰在内存驻留时间最长的页面
      - 最近最久未使用淘汰算法（LRU算法）：淘汰最久没有被使用的页面
      - 最不经常使用淘汰算法（LFU算法）：淘汰一段时间内，访问次数最少的页面。
  - 段式存储管理：段式存储管理要求每个作业的地址空间按照程序自身的逻辑划分为若干段，每个段都有一个唯一的内部段号。
  - 段页式存储管理：在段页式存储中，每个分段又被分成若干个固定大小的页。

- 地址重定位：逻辑地址转换成物理地址，这个过程称为地址重定位

  - 静态重定位：在装入作业时，装入程序把指令地址和数据地址全部转化为绝对地址，转换工作在作业执行前一次性完成。

  - 动态重定位：在程序执行时，配合重定位寄存器将相对地址转换为主存的绝对地址，更加灵活有效，不要给程序分配大片的连续空间，也能提供一个比主存大得多的地址空间

- 缓存

  - LRU最近最少使用算法：根据数据的历史访问记录来进行淘汰数据，其核心思想是“如果数据最近被访问过，那么将来被访问的几率也更高”。

  - 主存到缓存的映射策略

    - 直接映射：一个内存地址能被映射到的Cache line是固定的

    - 全相联映射：主存中的一个地址可被映射进任意cache line

    - 组相联映射：将缓存分组，组内包含多块，组间采用直接映射，组内采用全相联映射

      例题帮助理解组相联映射，可跳过直接看解答。

    > 例题：容量为64块的Cache采用组相联方式映像，字块大小为128字节，每4块为一组，若主容量为4096块，且以字编址，那么主存地址为**（19）**位，主存区号为**（6）**位。
    >
    >
    >
    > 组相联的地址构成为：区号+组号+块号+块内地址,主存的每个分区/组大小与整个Cache大小相等，故此主存需要分的区数为：4096/64=64，因为2^6＝64，因此需要6位来表示区号。每4块为一组，故共有组数 64/4 = 16 ，因为2^4＝16，因此需要4位表示组号。每组4块，故表示块号需要2位。
    >
    > 块内地址共128字节，2^7＝128，所以块内地需要7位表示。所以：主存地址的位数＝6+4+2+7 ＝ 19

    假设某计算机按字编址，Cache有4个行，Cache和主存之间交换的块大小为1个字。若Cache的内容初始为空，采用2路组相联映射方式和LRU替换策略。访问的主存地址依次为0,4,8,2,0,6,8,6,4,8时，命中Cache的次数是**（3）**。

    2路组相联映射，即缓存分为两组，又缓存有4行，因而每组内分为2行，根据前面例题的启示，主存与缓存的**直接映射关系**应当为：组号= 主存地址%4/2（对4求余忽略区号），组内行号采用**全相联映射**，结合LRU算法，，那么我们可以得到下表：



    | 组号 | 缓存行号 | 0       | 4       | 8              | 2       | 0              | 6       | 8     | 6     | 4              | 8     |
    | ---- | -------- | ------- | ------- | -------------- | ------- | -------------- | ------- | ----- | ----- | -------------- | ----- |
    | 0组  | 0        | 0       | 0       | 8              | 8       | 8              | 8       | 8*    | 8*    | 8*             | 8**   |
    | 0组  | 1        |         | 4       | 4              | 4       | 0              | 0       | 0     | 0     | 4              | 4     |
    | 1组  | 0（3）   |         |         |                | 2       | 2              | 2       | 2     | 2     | 2              | 2     |
    | 1组  | 1（4）   |         |         |                |         |                | 6       | 6     | 6*    | 6*             | 6*    |
    | 备注 |          | 0%4/2=0 | 4%4/2=0 | 8%4/2=0，替换0 | 2%4/2=1 | 0%4/2=0，替换4 | 6%4/2=1 | 8命中 | 6命中 | 4%4/2=0，替换0 | 8命中 |

  - 替换策略

    - LRU最近最少使用

  - Cache的总容量包括：存储容量和标记阵列容量（有效位、标记位、一致性维护位和替换算法控制位）

    - 标记阵列中的有效位和标记位是一定有的，而一致性维护位（脏位）和替换算法控制位的取舍标准是看题眼

      ```
      假定主存地址为32位，按字节编址，主存和Cache之间采用直接映射方式，主存块大小为4个字，每字32位，采用回写（Write Back）方式，则能存放4K字数据的Cache的总容量的位数至少是【148K】
      
      按字节编址，块大小为4×32bit=16B=24B，则“字块内地址”占4位；“能存放4K字数据的Cache”即Cache的存储容量为4K字（注意单位），则Cache共有1K=2^10个Cache行，则Cache字块标记占10位；则主存字块标记占32-10-4=18位。
      
      题目中，明确说明了采用写回法，则一定包含一致性维护位，而关于替换算法的词眼题目中未提及，所以不予考虑。从而每个Cache行标记项包含18+1+1=20位，则标记阵列容量为：2^10*20位=20K位，存储容量为：4K*32位=128K位，则总容量为：128K+20K=148K位。
      
      ```

- 共享

  - 同步信号量的初值一般设为0；互斥信号量的初值一般设为1；
  - 临界资源是一次仅允许一个进程使用的共享资源
  - **每个进程中访问临界资源的那段程序称为临界区**
  - 活跃度失败
    - 饥饿：指线程需要访问的资源 被永久拒绝 ，以至于不能再继续进行。解决饥饿问题需要平衡线程对资源的竞争，如线程的优先级（如果每次来就执行优先级高的，那么优先级低的可能永远执行不到）、任务的权重、执行的周期等。
    - 死锁：互相等着对方释放资源，结果谁也得不到
    - 活锁：指线程虽然没有被阻塞，但由于某种条件不满足，一直尝试重试却始终失败。

- 字节在内存中存储方式

  小端法(Little-Endian)就是低位字节排放在内存的低地址端(即该值的起始地址),高位字节排放在内存的高地址端

  大端法(Big-Endian)就是高位字节排放在内存的低地址端(即该值的起始地址),低位字节排放在内存的高地址端

  多字节数值在发送之前,在内存中因该是以大端法存放的，但X86 系列 CPU都是 little－endian 的，

  UDP/TCP/IP协议规定:把接收到的第一个字节当作高位字节看待,这就要求发送端发送的第一个字节是高位字节

- 软硬链接

  http://www.ruanyifeng.com/blog/2011/12/inode.html

- 实例模式和保护模式

#### 进程与线程

- 进程分为基本的三个状态：运行、就绪、阻塞/**等待**
  - 高优先级抢占CPU，状态由运行->就绪
  - 阻塞的进程等待某一条件满足，一旦满足，阻塞->就绪
  - 时间片结束时，状态由运行->就绪
  - 自旋锁（spinlock）是一种保护临界区最常见的技术，没有获得自旋锁的进程在获取锁之前处于忙等（**阻塞**状态）

- 父子进程间

  - 二者并**不共享地址空间**
  - 子进程得到的是除了**代码段**是与父进程共享的以外，其他所有的都是得到父进程的一个**副本**，子进程的所有资源都继承父进程
  - 

- 线程与进程

  - 进程：

    - 进程控制块(PCB)是用来记录进程状态及其他相关信息的数据结构,PCB是进程存在的唯一标志，PCB存在则进程存在。系统创建进程时会产生一个PCB，撤销进程时，PCB也自动消失。

  - 线程有自己的栈（局部变量），但是共享text、static和堆

  - **线程之间没有单独的地址空间**

  - 进程间通信方式

    1.管道

    2.信号量

    3.消息队列

    4.信号

    5.共享内存

    6.套接字

    7.文件和记录锁定

  - 线程和进程的区别联系
    - 进程：子进程是父进程的复制品。子进程获得父进程数据空间、堆和栈的复制品。
    - 线程：相对与进程而言，线程是一个更加接近于执行体的概念，它可以与同进程的其他线程共享数据，但**拥有自己的栈空间**（保存其运行状态和局部自动变量），拥有独立的执行序列。   
    - 根本区别就一点：多进程每个进程有自己的地址空间(address space)，线程则共享地址空间。所有其它区别都是由此而来的： 
      1、速度：线程产生的速度快，线程间的通讯快、切换快等，因为他们在同一个地址空间内。 
      2、资源利用率：线程的资源利用率比较好也是因为他们在同一个地址空间内。 
      3、同步问题：线程使用公共变量/内存时需要使用同步机制还是因为他们在同一个地址空间内
  - 调用线程的sleep()方法,可以使比当前线程优先级低的线程获得运行机会
  - yield()方法使得当前的线程让出CPU的使用权，以使得比该线程优先级相同或更高的线程有机会运行。该线程在让出CPU使用权之后可能再次被选中，因此yield()方法可能会不起作用(这也说明了yield()方法不会使得比当前线程优先级低的线程运行)。
  - 

- 从n个数中找出最小的k个数

  对前k个数，建立最大堆，对于后面N-k个数，依次和最大堆的最大数比较，如果小于最大数，则替换最大数，并重新建立最大堆。时间复杂度为O(N*logk)。当k和N都很大时，这种算法比前两种算法要快很多。

- ==比较内存地址，equals方法比较值

### C++

- 构造函数和析构函数

### 数据结构

- 红黑树

  性质1. 节点是红色或黑色。

  性质2. 根节点是黑色。

  性质3 每个叶节点（NIL节点，空节点）是黑色的。

  性质4 每个红色节点的两个子节点都是黑色。(从每个叶子到根的所有路径上不能有两个连续的红色节点)

  性质5. 从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点。

### 数据库

#### 数据库设计范式

- 函数依赖

  同一张表中存在部分函数依赖和传递函数依赖的话，会出现数据数据冗余量大，插入异常，删除异常，修改异常等问题

  - 完全函数依赖若X->Y，且对于任何X的子集X‘，Y均不函数依赖于X‘，即X‘->Y不成立

  - 部分函数依赖，X->Y,但Y并不完全函数依赖于X（存在冗余主码，显然码加上任何属性不会构成 新的候选码）

  - 传递函数依赖：传递函数依赖会导致数据冗余和异常。传递函数依赖的Y和Z子集往往同属于某一个事物，因此可将其合并放到一个表中。

    第二范式可以减少部分冗余，非主属性可以分表存储

- 第一范式：符合1NF的关系中的每个属性都不可再分，即字段是最小的单元，不可再分

  关系模式（schema）-关系

- 第二范式：消除**非主属性**对于**码**的**部分函数依赖**。

  - 码：k为表中的一个属性或者属性组，若除k外的所有属性**完全函数依赖**于k，那么称k为候选码（简称码），一张表可以有超过一个码，为了方便，通常会选择一个作为主码【一个码添加其他属性便不再构成码，违反完全函数依赖设定】
  - 非主属性：包含在任何一个码中的属性为主属性

- 第三范式：3NF在2NF的基础之上，消除了**非主属性**对于**码**的**传递函数依赖**

  非主属性不能完全函数依赖于非主属性

  解决删除异常、修改异常和插入异常，数据冗余更少

  由此可见，符合3NF要求的数据库设计，**基本**上解决了数据冗余过大，插入异常，修改异常，删除异常的问题。

- BCNF

  满足3NF的设计仍可能存在插入，修改与删除异常的问题，这是因为存在主属性对码存在部分函数依赖或传递函数依赖关系

#### 事务

不一致现象：

脏读，不可重复读，幻读，具体参见设计数据密集型系统事务一章

隔离级别：

- 读未提交
- 读已提交
- 可重复读
- 序列化

#### 数据库引擎

存储引擎



#### 数据库索引

- 调用函数后，就不会用到索引，查询速度变慢

  - 增加索引会增加磁盘占用
  - 建立索引可以提升查询速度，即读速度；但在一定程度上降低写速度
  - 数据库一般使用B*树作为索引
  - 删除数据需要调整索引，所以会降低效率

- **复合索引可以只使用复合索引中的一部分，但必须是由最左部分开始，且可以存在常量。**

  选择题中常用来考察不同查询条件的效率问题

### java陷阱

- String和String Buffer实现

- 子类继承父类的方法是，控制符必须大于或等于父类的访问控制符

- 重写方法不能比被重写方法限制有更严格的访问级别

- 重写方法不能抛出新的异常或者比被重写方法声明的检查异常更广的检查异常。但是可以抛出更少，更有限或者不抛出异常。

- 接口定义

  - static方法在interface中要有body
  - private修饰的方法不可以出现在interface中

- start启动线程，在某个时间会调用run方法，而直接调用run方法则会直接顺序执行了

- java回收算法

  整个java的垃圾回收是新生代和年老代的协作，这种叫做分代回收。

  - 两个基本的回收算法

    - 标记算法

      一块区域，标记可达对象（可达性分析），然后回收不可达对象，会出现碎片，那么引出

      - 标记整理算法

        多了碎片整理，整理出更大的内存放更大的对象

    - 复制算法

      两个区域A和B，初始对象在A，继续存活的对象被转移到B。此为新生代最常用的算法

  - 新生代和永久代

    - 新生代：初始对象，生命周期短的
    - 永久代：长时间存在的对象

  - 不同的收集器

    - 新生代收集器使用的收集器：Serial、PraNew、Parallel Scavenge

      - Serial New （复制算法）针对新生代的单线程收集器，采用的是复制算法
      - Parallel New（**ParNew**）**(停止-复制算法)**新生代收集器，可以认为是Serial收集器的多线程版本,在多核CPU环境下有着比Serial更好的表现。
      - Parallel Scavenge（复制算法）并行收集器针对新生代，采用复制收集算法

    - 老年代收集器使用的收集器：Serial Old、Parallel Old、CMS

      - Serial Old（标记整理）老年代单线程收集器，Serial收集器的老年代版本。
      - **Parallel Old收集器(标记－整理算法)**
      - **CMS(Concurrent Mark Sweep)收集器（标记-清理算法）**

  **综上：新生代基本采用复制算法，老年代采用标记整理算法。cms采用标记清理。**

### 网络

- Http会话的四个过程:

  建立连接，发送请求，返回响应，关闭连接。

- TIME_WAIT

  客户端主动关闭连接时，会发送最后一个ack后，然后会进入TIME_WAIT状态，再停留2个MSL时间(后有MSL的解释)，进入CLOSED状态。

- OSI七层结构

  - 表示层解决用户信息的语法表示
  - 会话层对数据传输进行管理
  - 每一层传递的数据包

  ​    **物理层：**通过媒介传输比特,确定机械及电气规范（比特Bit）

  ​    **数据链路层**：将比特组装成帧和点到点的传递（帧Frame）

  ​    **网络层**：负责数据包从源到宿的传递和网际互连（包PackeT）

  ​    **传输层**：提供端到端的可靠报文传递和错误恢复（段Segment）

  ​    **会话层**：建立、管理和终止会话（会话协议数据单元SPDU）

  ​    **表示层**：对数据进行翻译、加密和压缩（表示协议数据单元PPDU）

  ​    **应用层**：允许访问OSI环境的手段（应用协议数据单元APDU）

​	

- post和get

  | 操作方式 | 数据位置 | 明文密文 | 数据安全 | 长度限制         | 应用场景 |
  | -------- | -------- | -------- | -------- | ---------------- | -------- |
  | GET      | HTTP包头 | 明文     | 不安全   | 长度较小         | 查询数据 |
  | POST     | HTTP正文 | 可明可密 | 安全     | 支持较大数据传输 | 修改数据 |

- MTP：简单邮件传输协议，使用TCP连接，端口号为25，
  SNMP：简单网络管理协议，使用UDP 161端口，

- 网络层协议包括： IP协议、ICMP协议、ARP协议、RARP协议；
  传输层协议包括：TCP协议、UDP协议

- 物理层：RJ45、CLOCK、IEEE802.3 

  数据链路：PPP、FR、HDLC、VLAN、MAC 

  网络层：IP、ICMP、ARP、RARP、OSPF、IPX、RIP、IGRP

  传输层：TCP、UDP、SPX

  会话层：NFS、SQL、NETBIOS、RPC

  表示层：JPEG、MPEG、ASII

  应用层：FTP（文件传送协议）、Telenet（远程登录协议）、DNS（域名解析协议）、SMTP（邮件传送协议），POP3协议（邮局协议），HTTP协议， SNMP协议， TFTP。

### 设计模式


A单例模式没有提高扩展性

B工厂方法实现松耦合，可以提高扩展性

C适配器模式可以将一个接口转换成另一个接口，方便引入外部接口

D装饰者模式可以扩展接口功能

### 计算机操作系统

https://wizardforcel.gitbooks.io/think-os/content/ch11.html

1. 编译
   1. 编译和解释语言
   2. 静态类型
   3. 编译过程
   4. 目标代码
   5. 汇编代码
   6. 预处理
   7. 理解错误
2. 进程
   1. 抽象和虚拟化
   2. 隔离
   3. UNIX进程
3. 虚拟内存
   1. 简明信息理论
   2. 内存和存储
   3. 地址空间
   4. 内存段
   5. 静态局部变量
   6. 地址转换
4. 文件和文件系统
   1. 磁盘性能
   2. 磁盘元数据
   3. 块分配
   4. 一切都是一个文件？
5. 更多的位和字节
   1. 表示整数
   2. 位运算符
   3. 表示浮点型数
   4. 合并和内存错误
   5. 表示字符串
6. 内存管理
   1. 内存错误
   2. 内存泄露
   3. 实现
7. 缓存
   1. 程序如何运行
   2. 缓存性能
   3. 局部性
   4. 衡量缓存性能
   5. 缓存性能编程
   6. 内存层次结构
   7. 缓存策略
   8. 分页
8. 多任务
   1. 硬件状态
   2. 上下文转换
   3. 进程的生命周期
   4. 调度
   5. 实时调度
9. 线程
   1. 创建线程
   2. 同上
   3. 加入线程
   4. 同步错误
   5. 互斥
10. 条件变量
    1. 工作队列
    2. 生产者和消费者
    3. 互斥
    4. 条件变量
    5. 条件变量的实现
11. c中的信号量(Semaphores)
    1. POSIX信号量
    2. 生产者和消费者的信号量
    3. 创建自己的信号量



1. #### 编译

   1. ##### 编译和解释语言

   2. ##### 静态类型

      - “静态”指那些在编译时发生的事情，而“动态”指在运行时发生的事情。
      - 在编译语言中，变量的名称只存在于编译时，而不是运行时。编译器为每个变量选择一个位置，并记录这些位置作为所编译程序的一部分。变量的位置被称为“地址”。在运行期间，每个变量的值都储存在它的地址处，但是变量的名称完全不会储存（除非它们由于调试目的被编译器添加）

   3. ##### 编译过程

      - 预处理

      - 解析

        将程序读进内存中，构建语法树

      - 静态检查

        检查类型

      - 代码生成

      - 链接

      - 优化

   4. ##### 目标代码

      机器码

   5. ##### 汇编代码

      汇编代码——机器代码的可读形式

   6. ##### 预处理

   7. ##### 理解错误

      段错误——读写内存的不正确位置——

2. #### 进程

   1. ##### 抽象和虚拟化

      - 抽象

        抽象是复杂事物的简单表示

      - 虚拟化

        在虚拟化的语境中，我们通常把真实发生的事情叫做“物理的”，而把虚拟上发生的事情叫做“逻辑的”或者“抽象的”。

   2. ##### 隔离

      操作系统最重要的目标之一，就是将每个进程和其它进程隔离，使程序员不必考虑每个可能的交互情况。提供这种隔离的软件对象叫做进程（Process）。 

      进程是包含以下数据的对象：

      - 程序文本
      - 数据相关数据
      - 任何等待中的I/O状态
      - 程序的硬件状态

      进程和程序的关系

      - 通常一个进程运行一个程序，但是对于进程来说，加载并运行新的程序也是可能的。 
      - 多于一个进程中运行相同的程序，这非常常见。这种情况下，各个进程**共享程序文本**，但是**拥有不同的数据和硬件状态**。

   3. ##### UNIX进程

3. #### 虚拟内存

   1. ##### 简明信息理论

   2. ##### 内存和存储器

   3. ##### 地址空间

      - 主存中的每个字节都由一个“物理地址”整数所指定，物理地址的集合叫做物理“地址空间”。它的范围通常为0到`N-1`，其中`N`是主存的大小。
      - 虚拟内存

   4. ##### 内存段

      一个运行中进程的数据组织为4个段(理解堆栈的差异)：

      - text
      - static
      - stack
      - heap

      这些段的组织方式部分取决于编译器，部分取决于操作系统。不同的操作系统中细节可能不同，但是下面这些是共同的：

      - text段靠近内存“底部”，即接近0的地址
      - static段通常刚好在text段上方
      - stack段靠近内存顶部，即接近虚拟地址空间的最大地址。在扩张过程中，它向低地址的方向增长
      - heap通常在static段的上面。在扩张过程中，它向高地址的方向增长

      通过下面这段程序来看不同段在内存中的实际地址

      ```c
      #include <stdio.h>
      #include <stdlib.h>
      
      int global;
      
      int main ()
      {
          int local = 5;
          int local2 = 5;
      
          void *p = malloc(128);
          void *p2 = malloc(128);
      
          printf ("Address of main is %p\n", main);
          printf ("Address of global is %p\n", &global);
          printf ("Address of local is %p\n", &local);
          printf ("Address of local2 is %p\n", &local2);
          printf ("Address of p is %p\n", p);
          printf ("Address of p2 is %p\n", p2);
          
          return 0;
      }
      ```

      输出

      ```shell
      Address of main is 0x104ee7e20
      Address of global is 0x104ee8020
      Address of local is 0x7ffeead18668
      Address of local2 is 0x7ffeead18664
      Address of p is 0x7fbc38c02890
      Address of p2 is 0x7fbc38c02910
      ```

      - main是函数的名称，当它用作变量时，它指向main中第一条机器语言指令的地址，即认为它在text段中
      - global是一个全局变量，它在static段内
      - local是一个局部变量，它在栈上，同时比较local和local2的地址，可以发现，地址从高地址向低地址方向增长
      - p持有malloc所返回的地址，它指向**堆区**所分配的空间，比较p和p2在内存中的地址，可以发现，地址从低地址向高地址方向增长

   5. ##### 静态局部变量

      静态局部变量在程序启动时初始化，和全局变量一起分配在static段中

   6. ##### 地址转换

4. #### 文件和文件系统

   1. ##### 磁盘性能

      相比cpu的始终周期来说很慢，所以通常在进行io传输时，cpu会切换到别的进程

   2. ##### 磁盘元数据

   3. ##### 块分配

   4. ##### 一切都是一个文件？

5. #### 更多的位和字节

   1. ##### 表示整数

      计算机以补码形式表示数字

   2. ##### 位运算符

      通常，`&`用于清除位向量中的一些位，`|`用于设置位，`^`用于反转位

   3. ##### 浮点数的表示

      在32位的标准中，最左边那位是符号位，`s`。接下来的8位是指数`q`，最后的23位是系数`c`。浮点数的值为：

      ```shell
      (-1) ** s * c * 2 ** q
      ```

   4. ##### 合并和内存错误

   5. ##### 表示字符串

6. #### 内存管理

   1. ##### 内存错误

   2. ##### 内存泄露

      如果你分配了一块内存并且没有释放它，就会产生“内存泄漏”

   3. ##### 实现

7. #### 缓存

   1. ##### 程序如何运行

   2. ##### 缓存性能

      - 缓存命中率h：内存访问时，在缓存中找到数据的比例

      - 缓存缺失率m：内存访问中需要访问内存的比例

        缓存的“命中率”`h`，是内存访问时，在缓存中找到数据的比例。“缺失率”`m`，是内存访问时需要访问内存的比例。如果`Th`是处理缓存命中的时间，`Tm`是缓存未命中的时间，每次内存访问的平均时间是：

        ```
        h * Th + m * Tm
        ```

        同样，我们可以定义“缺失惩罚”，它是处理缓存未命中所需的额外时间，`Tp = Tm - Th`，那么平均访问时间就是：

        ```
        Th + m * Tp
        ```

   3. ##### 局部性

      程序使用相同数据多于一次的倾向叫做“时间局部性”。使用相邻位置的数据的倾向叫做“空间局部性”。 

   4. ##### 衡量缓存性能

      > **单位（Unit）**
      > 位 bit b
      > 字节 byte B
      > 字 word W
      > 字长 word size/length
      > CPU 一次并行处理的二进制位数（通常为 32/64 位）
      > **32 位 CPU：1 Word = 4 Bytes = 32 bits**
      > **64 位 CPU：1 Word = 8 Bytes = 64 bits**
      > **地址的三个域**
      > 设块大小（Cache Block Size）为 B，Cache 组数（Cache Sets）为 S
      > 块数 = Cache 容量（Cache Size）／块大小（Cache Block Size）
      > Cache 组数（Cache Sets） = 块数（Cache Blocks）／路数（Way）
      > tag = 地址总位数（单位为 bits） - index - offset bits
      > index = log2S bits（全相联(full-associative)时，组数(Cache Sets)为0）
      > offset = log2B bits（B 单位为 Byte）
      > PS: Cache Block 与 Cache Line 同义

      一个有趣的练习

      估算电脑缓存大小,缓存块大小，如果数组比缓存大小更小，或步长小于块的大小，我们认为会有良好的缓存性能。如果数组大于缓存大小，并且步长较大时，性能只会下降。

   5. ##### 缓存性能编程

      缓存感知

   6. ##### 存储器层次结构

      | 设备   | 访问时间 | 通常大小 | 成本        |
      | ------ | -------- | -------- | ----------- |
      | 寄存器 | 0.5 ns   | 256 B    | ?           |
      | 缓存   | 1 ns     | 2 MiB    | ?           |
      | DRAM   | 10 ns    | 4 GiB    | $10 / GiB   |
      | SSD    | 10 µs    | 100 GiB  | $1 / GiB    |
      | HDD    | 5 ms     | 500 GiB  | $0.25 / GiB |
      | 磁带   | minutes  | 1–2 TiB  | $0.02 / GiB |

      - 寄存器的数量和大小取决于体系结构的细节。当前的计算机拥有32个通用寄存器，每个都可以储存一个“字”。在32位计算机上，一个字为32位，4个字节。64位计算机上，一个字为64位，8个字节。所以寄存器文件的总容量是100~300字节。 
      - 这些技术构成了“存储器体系结构”。结构中每一级都比它上一级大而缓慢。某种意义上，每一级都作为其下一级的缓存。 你可以认为主存是持久化储存在SSD或HDD上的程序和数据的缓存。并且如果你需要处理磁带上非常大的数据集，你可以用硬盘缓存一部分数据。

   7. ##### 缓存策略

      - 需要强调四个缓存的基本问题：

        - 谁在层次结构中上移或下移数据？在结构的顶端，寄存器通常由编译器完成分配。CPU上的硬件管理内存的缓存。在执行程序或打开文件的过程中，用户可以将存储器上的文件隐式移动到内存中。但是操作系统也会将数据从内存移动回存储器。在层次结构的底端，管理员在磁带和磁盘之间显式移动数据。
        - 移动了什么东西？通常，在结构顶端的块大小比底端要小。在内存的缓存中，通常块大小为128B。内存中的页面可能为4KiB，但是当操作系统从磁盘读取文件时，它可能会一次读10或100个块。
        - 数据什么时候会移动？在多数的基本的缓存中，数据在首次使用时会移到缓存。但是许多缓存使用一些“预取”机制，也就是说数据会在显式请求之前加载。我们已经见过预取的一些形式了：在请求其一部分时加载整个块。
        - 缓存中数据在什么地方？当缓存填满之后，我们不把一些东西扔掉就不可能放进一些东西。理想化来说，我们打算保留将要用到的数据，并替换掉不会用到的数据。

      - 缓存策略

        - LRU，最近最少使用

          从缓存中移除最久未使用的数据块

   8. ##### 页面调度

      在带有虚拟内存的系统中，操作系统可以将页面在存储器和内存之间移动。

      进程和程序的关系：

      - 系统开辟进程来执行程序，同一个程序可以被多个进程执行

8. #### 多任务

   CPU包含多个核心，也就是说它可以同时运行多个进程。而且，每个核心都具有“多任务”的能力，也就是说它可以从一个进程快速切换到另一个进程，创造出同时运行许多进程的幻象。

   1. ##### 硬件状态

   2. ##### 上下文转换

   3. ##### 进程的生命周期

   4. ##### 调度

   5. ##### 实时调度

9. #### 线程

   - 在单一进程中，所有线程都共享相同的`text`段，所以它们运行相同的代码。但是不同线程通常运行代码的不同部分
   - 同时它们间共享相同的static段和堆区
   - **每个线程都有自己的栈**

   1. ##### 创建线程

   2. ##### 同上

   3. ##### 回收线程

      join等待线程执行完毕并回收

   4. ##### 同步错误

      子线程不加限制访问共享变量，每次你运行这个程序的时候，**线程都会在不同时间点上中断**，或者**调度器可能选择不同的线程来运行**，所以时间序列和结果都是不同的

   5. ##### 互斥

10. #### 条件变量

   1. ##### 工作队列

      在一些多线程的程序中，线程被组织用于执行不同的任务。通常它们使用队列来相互通信，其中一些线程叫做“生产者”，向队列中放入数据，另一些线程叫做“消费者”，从队列取出数据。

   2. ##### 生产者和消费者

      当队列为空时，阻塞消费者；当队列满时，阻塞生产者

   3. ##### 互斥

   4. ##### 条件变量

   5. ##### 条件变量的实现

11. #### c中的信号量(Semaphores)

    1. ##### POSIX信号量

       - 信号量是用于使线程协同工作而不互相影响的数据结构。
         - 信号量的类型表现为结构体
         - 关于此结构体的api：初始化(make_semaphore)，阻塞(semaphore_wait) ，发送(semaphore_signal)
       - 当你将信号量用作互斥体时，通常需要将它初始化为1，来表示互斥体是未锁的。

    2. ##### 生产者和消费者的信号量

    3. ##### 创建自己的信号量

 

### 网络工程师们的计算机体系结构





| TCP                                                          | CPU                                                   |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| TCP发送一连串的数据包                                        | CPU处理一系列指令                                     |
| TCP数据包最终被收到                                          | CPU指令最终会过期                                     |
| TCP会根据窗口大小发送一些列的几个数据包，而不会等待第一个数据包被接收后才行动 | CPU同时处理多个指令，而不是等待第一个指令执行完才行动 |
|                                                              | 指令引用没有缓存在内存地址时                          |
|                                                              |                                                       |
|                                                              |                                                       |
|                                                              |                                                       |
|                                                              |                                                       |
| 分类                                                         | 分类描述                                              |





| 分类 | 分类描述                                       |
| ---- | ---------------------------------------------- |
| 1**  | 信息，服务器收到请求，需要请求者继续执行操作   |
| 2**  | 成功，操作被成功接收并处理                     |
| 3**  | 重定向，需要进一步的操作以完成请求             |
| 4**  | 客户端错误，请求包含语法错误或无法完成请求     |
| 5**  | 服务器错误，服务器在处理请求的过程中发生了错误 |



| 状态码 | 状态码英文名称                  | 中文描述                                                     |
| ------ | ------------------------------- | ------------------------------------------------------------ |
| 100    | Continue                        | 继续。客户端应继续其请求                                     |
| 101    | Switching Protocols             | 切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议 |
|        |                                 |                                                              |
| 200    | OK                              | 请求成功。一般用于GET与POST请求                              |
| 201    | Created                         | 已创建。成功请求并创建了新的资源                             |
| 202    | Accepted                        | 已接受。已经接受请求，但未处理完成                           |
| 203    | Non-Authoritative Information   | 非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本 |
| 204    | No Content                      | 无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档 |
| 205    | Reset Content                   | 重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域 |
| 206    | Partial Content                 | 部分内容。服务器成功处理了部分GET请求                        |
|        |                                 |                                                              |
| 300    | Multiple Choices                | 多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择 |
| 301    | Moved Permanently               | 永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替 |
| 302    | Found                           | 临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI |
| 303    | See Other                       | 查看其它地址。与301类似。使用GET和POST请求查看               |
| 304    | Not Modified                    | 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源 |
| 305    | Use Proxy                       | 使用代理。所请求的资源必须通过代理访问                       |
| 306    | Unused                          | 已经被废弃的HTTP状态码                                       |
| 307    | Temporary Redirect              | 临时重定向。与302类似。使用GET请求重定向                     |
|        |                                 |                                                              |
| 400    | Bad Request                     | 客户端请求的语法错误，服务器无法理解                         |
| 401    | Unauthorized                    | 请求要求用户的身份认证                                       |
| 402    | Payment Required                | 保留，将来使用                                               |
| 403    | Forbidden                       | 服务器理解请求客户端的请求，但是拒绝执行此请求               |
| 404    | Not Found                       | 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面 |
| 405    | Method Not Allowed              | 客户端请求中的方法被禁止                                     |
| 406    | Not Acceptable                  | 服务器无法根据客户端请求的内容特性完成请求                   |
| 407    | Proxy Authentication Required   | 请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权 |
| 408    | Request Time-out                | 服务器等待客户端发送的请求时间过长，超时                     |
| 409    | Conflict                        | 服务器完成客户端的PUT请求是可能返回此代码，服务器处理请求时发生了冲突 |
| 410    | Gone                            | 客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置 |
| 411    | Length Required                 | 服务器无法处理客户端发送的不带Content-Length的请求信息       |
| 412    | Precondition Failed             | 客户端请求信息的先决条件错误                                 |
| 413    | Request Entity Too Large        | 由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息 |
| 414    | Request-URI Too Large           | 请求的URI过长（URI通常为网址），服务器无法处理               |
| 415    | Unsupported Media Type          | 服务器无法处理请求附带的媒体格式                             |
| 416    | Requested range not satisfiable | 客户端请求的范围无效                                         |
| 417    | Expectation Failed              | 服务器无法满足Expect的请求头信息                             |
|        |                                 |                                                              |
| 500    | Internal Server Error           | 服务器内部错误，无法完成请求                                 |
| 501    | Not Implemented                 | 服务器不支持请求的功能，无法完成请求                         |
| 502    | Bad Gateway                     | 充当网关或代理的服务器，从远端服务器接收到了一个无效的请求   |
| 503    | Service Unavailable             | 由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中 |
| 504    | Gateway Time-out                | 充当网关或代理的服务器，未及时从远端服务器获取请求           |
| 505    | HTTP Version not supported      | 服务器不支持请求的HTTP协议的版本，无法完成处理               |



### 计算机组成原理

存储程序概念是冯·诺依曼等人在1946年6月首先提出来的，它可以简要地概括为以下 几点: 

1. 计算机(指硬件)应由运算器、存储器、控制器、输入设备和输出设备五大基本部 件组成; 
2. 计算机内部采用二进制来表示指令和数据;
3. 将编好的程序和原始数据事先存入存储器中，然后再启动计算机工作，这就是存储程序的基本含义。

![](/media/files/images/sc_51.png)

- 运算器和控制器合称为中央处理器CPU

- 中央处理器和主存储器（内存储器）一起组成主机部分

- 三级存储系统

  - 高速缓冲存储器

    存取速度比主存更快，但是容量更小

  - 主存储器

  - 辅助存储器

  三级系统可再划分为两个层次：

  - Cache存储系统

    高速缓存和主存

  - 虚拟存储系统

    主存和辅村

- 计算机执行过程实例

  ![](/media/files/images/sc_52.png)

微服务中的关键技术挑战

- 服务间通信
  - http
  - 异步消息
- 分布式数据管理



缓存-遗漏，乱序执行，流水线执行



编程基本功扎实，掌握C/C++/JAVA等开发语言、常用算法和数据结构；<br/>熟悉TCP/UDP网络协议及相关编程、进程间通讯编程；<br/>了解Python、Shell、Perl等脚本语言；<br/>了解MYSQL及SQL语言、编程，了解NoSQL,&nbsp;key-value存储原理；<br/>全面、扎实的软件知识结构，掌握操作系统、软件工程、设计模式、数据结构、数据库系统、网络安全等专业知识；<br/>了解分布式系统设计与开发、负载均衡技术，系统容灾设计，高可用系统等知识。

### 后台开发面试

- TCP/UDP

  三次招手

  四次挥手

  redis

  redis关键的一个特性：redis的单线程的

  - 面试题
    - redis数据结构
      - 分布式锁的可重入性

  - 应用 布隆过滤器

### 常用linux指令

### Python中的一些语法细节

#### 注意事项

- **永远不要使用小数比较结果来作为两者相等的判断依据**

  可以采取`abs(a-b) < 1e-7`作为二者相等的判断

- `==`操作符比较的是两个变量引用的对象是否具有相同的值

- `is`操作符比较的是两个变量是否引用的是同一个对象

### JAVA

，随即便问了下JAVA多线程技术，线程同步异步、线程安全问题，以及JAVA内存管理的机制
