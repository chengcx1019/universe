设计模式


> 提高开发能力，需要了解设计模式


## 引言

如果说数据结构和算法是教你如何写出高效代码，那设计模式讲的是如何写出可扩展、可读、可维护的高质量代码

不好的代码有什么特征：比如命名不规范、类设计不合理、分层不清晰、没有模块化概念、代码结构混乱、高度耦合

那什么是高质量的代码，问如何写出高质量的代码，也就等同于在问，如何写出**易维护**、**易读**、**易扩展**、**灵活**、**简洁**、**可复用**、**可测试的代码** 。其中，可维护性、可读性、可扩展性又是提到最多的、最重要的三个评价标准。

如何写出高质量的代码，需要掌握一些更加细化、更加能落地的编程方法论，包括面向对象设计思想、设计原则、设计模式、编码规范、重构技巧等。



### 面向对象

面向对象的四大特性：封装、抽象、继承、多态

针对面对对象设计思想，需要理解以下问题：

- 面向对象和面向过程编程的区别与联系
- 面向对象分析、面向对象设计、面向对象编程
- 接口和抽象类的区别以及各自的应用场景
- 基于接口而非实现编程的设计思想？这个人感觉也是一个需要权衡取舍的点， 《Java 编程思想》中提到，恰当的原则应该是优先选择类而不是借口。从类开始，如果接口的必需性变得非常明确，那么就进行重构。
- 多用组合少用继承的设计思想
- 面向过程的贫血模型和面向对象的充血模型

#### 面向对象的定义

##### **面向对象是什么** 

面向对象编程是一种编程范式或编程风格，它以类或对象作为组织代码的基本单元，并将封装、抽象、继承、多态四个特性，作为代码设计和实现的基石

面向对象编程语言是支持类或对象的语法机制，并有现成的语法机制，能方便的实现面向对象编程四大特性（封装、继承、抽象、多态）的编程语言

##### **面向对象分析和面向对象设计** 

类比需求分析和功能设计，只是以拆解为ER图，流程交互图的方式去做，这个过程中又涉及到 UML ，统一建模语言，通常用它来表达面向对象或设计模式的设计思路

#### 封装、抽象、继承、多态分别可以解决哪些编程问题

光知道它们的定义是不够的，还要知道每个特性存在的意义和目的

| 概念 | 定义                                                         | 语法/实现机制 | 意义                 |
| ------ | ---------------------------------------------------------- | ------------- | -------------------- |
| 封装 | 信息隐藏或者数据访问保护                                     | 访问权限控制  | 减少误用             |
| 抽象 | 如何隐藏方法的具体实现，让调用者只关心提供了哪些功能,无需知道如何实现的 | 接口类/抽象类 | 只关注功能点而非实现 |
| 继承 | 表示类之间的 is-a 的关系 | 特殊的语法机制支持，单继承/多继承 | 代码复用 |
| 多态 | 多态也常称作动态绑定，后期绑定或运行时绑定 | 父类对象引用子类对象（包括抽象类）/接口引用实现 | 提高代码的可扩展性和复用性 |

> 继承属于有利有弊的特性，继承的不足主要表现在以下两个方面，首先过度使用继承，继承层次太过复杂，就会导致代码可读性、可维护性变差，然后子类和父类高度耦合，修改父类的代码，会直接影响到子类。

#### 面向对象 VS 面向过程

使用支持面向对象语言开发并非就是面向对象编程

> 王争有时看问题比较透彻，但是有时某些想法还是比较低幼的，硬币的两面吧

1. 什么是面向过程编程与面向过程编程语言？

  面向过程和面向对象最基本的区别就是代码的组织方式不同，面向过程的风格是数据结构和方法的定义是分开的，而面向对象风格的代码被组织成一组类，方法和数据结构绑定在一起，定义在类中。

2. 面向对象编程相比面向过程编程有哪些优势？

3. 为什么说面向对象编程语言比面向过程编程语言更高级？

4. 有哪些看似是面向对象实际是面向过程风格的代码？

   滥用 getter、setter 方法

   滥用全局变量和全局方法：在单独定义 utils 和 bizhelper 时要问下自己，真的有必要单独定义一个类吗，是否可以把 utils 和 bizhelper 中的某些方法定义到其他类中

   定义数据和方法分离的类

5. 在面向对象编程中，为什么容易写出面向过程风格的代码？

   面向过程更符合通用思维逻辑

6. 面向过程编程和面向过程编程语言就真的无用武之地了吗？

#### 接口 VS 抽象类

接口是一种自上而下的设计思路，在编程的时候，一般都是先设计接口，再去考虑具体的实现

抽象类主要解决代码复用问题，接口主要解决耦合问题，隔离接口和具体的实现，提高代码的扩展性

#### 几条建议

#### 为什么基于接口而非实现编程？有必要为每个类都定义接口吗？

接口指的是一组协议或者约定

#### 是否需要为每个类定义接口

取决于可见的非稳定性，将接口和实现分离，封装不稳定的实现，暴露稳定的接口。	

#### 为何要多用组合少用继承？如何决定该用组合还是继承？

继承最大的问题在于，继承层次过深、继承关系过于复杂会影响到代码的可读性和可维护性。

从理论上来讲，通过组合、接口、委托三个技术手段，可以替换掉继承，但是，虽然鼓励多用组合少用继承，但组合也并不是完美，继承也并非一无是处。

使用继承和组合的权衡取决于复杂程度，如果继承关系浅，继承关系不复杂，可以大胆的使用继承。另外有一些特殊的场景，迫使我们必须使用继承，比如修改一个外部类的方法，只能继承这个外部类，并重写这个类方法。



### 设计原则

对于每一种设计原则，需要理解他的设计初衷，能解决哪些编程问题，有哪些应用场景。

很多时候评价标准是很模糊的，但是不管是应用设计原则还是设计模式，最终的目的还是提高代码的可读性、可扩展性、复用性、可维护性等。

- SOLID原则—— SRP（Single Responsibility Principle） 单一职责

  一个类或者模块只负责完成一个职责

- SOLID原则—— OCP（Open Closed Principle） 开闭原则

  添加一个新功能应该是，在已有代码的基础上扩展代码，而非修改已有代码

  最常用来提高代码扩展性的方法有：**多态、依赖注入、基于接口而非实现编程**

- SOLID原则—— LSP（Liskov Substitution Principle） 里氏替换原则

  子类对象能够替换程序中的父类对象出现在任何地方，并且保证原来程序的逻辑行为不变及正确性不被破坏

  里氏替换原则的要求比多态更严格，即里氏替换用到了多态的特性并且有自己额外的限制。

  里氏替换原则重点是说子类在设计的时候，要遵守父类行为约定，父类定义了函数的行为约定，那子类可以改变函数的内部实现逻辑，但不能改变函数原有的行为约定。行为的约定包括：

  1. 函数声明要实现的功能
  2. 对输入、输出、异常的约定

- SOLID原则—— ISP（Interface Segregation Principle） 接口隔离原则

  不要暴露无关接口给模块调用方

- SOLID原则—— DIP（Dependency Inversion Principle） 依赖反转原则

  我们常听到的是控制反转、依赖注入，那么依赖反转是啥玩意儿呢。

  控制反转（ IOC, Inversion Of Control）,是指流程的控制由人转移到框架，人只需要在预留的扩展点进行开发，框架来驱动整个程序流程的执行。控制反转不是一种具体的实现技巧，而是一个比较笼统的设计思想。

  依赖注入（DI, Dependency Injection）,是指不通过new的方式在类内部创建依赖类对象，而是将依赖的类对象在外部创建好以后，通过构造函数、函数参数等方式传递给类使用。

  依赖反转的核心观点是高层模块不要依赖低层模块，高层模块和低层模块应该**通过抽象来相互依赖**，另外，抽象不依赖具体实现细节，具体实现细节依赖抽象。

- DRY 原则、KISS原则、YAGNI原则、LOD法则

  - KISS（Keep It Simple and Stupid）原则：尽量保持简单
  - YAGNI（You Ain’t Gonna Need It）原则：你不会需要它，不要去设计当前用不到的功能，不要去编写当前用不到的代码，即不要过度设计，不过需要注意的是，不过度设计不代表完全不考虑扩展性。
  - DRY （Don’t Repeat Yourself）原则：重复的三种情况，实现逻辑重复，功能语义重复，代码执行重复。功能语义重复，但实现逻辑不同也算违反 DRY
  - LOD（Law of Demeter）：迪米特法则，又称「最小知识原则」，即每个模块只应该了解那些与它关系密切的模块的有限知识，换言之，可以表达为，“不该有直接依赖关系的类，不要有依赖；有依赖关系的类之间，尽量只依赖必要的接口“，利用这个原则实现代码的“高内聚，松耦合”，“高内聚，松耦合”是一个非常重要的设计思想，能够有效的提高代码的可读性和可维护性，缩小功能改动导致的代码改动范围。

- SOLID 原则之间的关系

  总的来说，单独应用SOLID的某一个原则并不能让收益最大化。应该把它作为一个整体来理解和应用，从而更好地指导你的软件设计。单一职责是所有设计原则的基础，开闭原则是设计的终极目标。里氏替换原则强调的是子类替换父类后程序运行时的正确性，它用来帮助实现开闭原则。而接口隔离原则用来帮助实现里氏替换原则，同时它也体现了单一职责。依赖倒置原则是过程式编程与OO编程的分水岭，同时它也被用来指导接口隔离原则，参考[你真的了解SOLID吗](https://insights.thoughtworks.cn/what-is-solid-principle/) 

### 设计模式


设计模式是针对软件开发过程中经常遇到的设计问题，总结出来的一套解决方案和设计思路。

23种经典的设计模式，可以分为三大类：创建型、结构型、行为型。

创建型：

常用的有：单例模式、工厂模式、建造者模式

不常用的有：原型模式

结构性：

常用的有：代理模式、桥接模式、装饰者模式、适配器模式

不常用的有：门面模式、组合模式、享元模式

行为型：

常用的有：观察者模式、模版模式、策略模式、职责链模式、迭代器模式、状态模式

不常用的有：访问者模式、备忘录模式、命令模式、解释器模式、中介模式



### 编程规范

《代码整洁之道》



### 规范与重构

重构是一种对软件内部结构的改善，目的是在不改变软件的可见行为的情况下，使其更易理解，修改成本更低，换言之，在保持功能不变的前提下，利用设计思想、原则、模式、编程规范等理论来优化代码，修改设计上的不足，提高代码质量。

#### 需要理清下面的问题：

- 重构是什么、为什么重构、重构什么

- 什么时候重构

- 如何重构

  - 单元测试和代码的可测试性
  - 大规模高层次重构和小规模低层次重构

  

### 这五者的关系：

- 面向对象编程的特性可以实现很多复杂的设计思路，是很多设计原则、设计模式等编码实现的基础
- 设计原则是很多设计模式的指导思想
- 设计模式是针对软件开发过程中的经常遇到的一些设计问题，总结出来的一套解决方案或设计思路
- 编码规范主要解决的是代码的可读性问题
- 重构作为保持代码质量不下降的有效手段，利用的是面向对象、设计原则、设计模式、编码规范这些理论

后面会重点就两个思想（基于接口而非实现编程、多用组合少用继承），五个原则（SOLID原则），及重构相关的话题进行重点分析



## 两个思想

> 详细内容见附件

## 五个原则

> 详细内容见附件

## 代码重构

重构是一种对软件内部结构的改善，目的是在不改变软件的可见行为的情况下，使其更易理解，修改成本更低。"重构不改变外部的可见行为"可以理解为"在保持功能不变的前提下，利用设计思想、原则、模式、编程规范等理论来优化代码，修改设计上的不足，提高代码质量"


### 编程规范

使用设计模式可以提高代码的可扩展性，但过度不恰当地使用，也会增加代码的复杂度，影响代码的可读性。

- 明确重构的目的（why）、对象（what）、时机（when）、方法（how）
- 保证重构不出错的技术手段：单元测试和代码的可测试性
- 两种不同规模的重构：大重构（大规模高层次）和小重构（小规模低层次）


### 规范与重构

重构是一种对软件内部结构的改善，目的是在不改变软件的可见行为的情况下，使其更易理解，修改成本更低，换言之，在保持功能不变的前提下，利用设计思想、原则、模式、编程规范等理论来优化代码，修改设计上的不足，提高代码质量。

#### 需要理清下面的问题：

- 重构是什么、为什么重构

- 重构什么

#### 重构四要素

- 目的
  1. 重构是时刻保证代码质量的一个极其有效的手段，不至于让代码腐化到无可救药的地步
  2. 优秀的代码或架构不是一开始就能完全设计好的，我们无法 100% 遇见未来的需求，也没有足够的精力、时间、资源为遥远的未来买单，所以，随着系统的演进，重构代码也是不可避免的
  3. 重构是避免过度设计的有效手段。在我们维护代码的过程中，真正遇到问题的时候，再对代码进行重构，能有效避免前期投入太多时间做过度的设计，做到有的放矢。
- 对象
- 时机
- 方法

- 什么时候重构

- 如何重构
  - 单元测试和代码的可测试性
  - 大规模高层次重构和小规模低层次重构


保留持续重构的意识

保持代码质量最好的方法还是打造一种良好的技术氛围，以此来驱动大家主动关注代码质量，持续重构代码

单元测试

什么是单元测试，为什么要写单元测试，如何编写单元测试，如何在团队中推行单元测试

为了保证重构不出错，有哪些能落地的技术手段


什么是代码的可测试性，如何写出可测试性好的代码

如何通过封装、抽象、模块化、中间层等解耦代码



```java

// 非依赖注入实现方式
public class Notification {
  private MessageSender messageSender;
  
  public Notification() {
    this.messageSender = new MessageSender(); //此处有点像hardcode
  }
  
  public void sendMessage(String cellphone, String message) {
    //...省略校验逻辑等...
    this.messageSender.send(cellphone, message);
  }
}

public class MessageSender {
  public void send(String cellphone, String message) {
    //....
  }
}
// 使用Notification
Notification notification = new Notification();

// 依赖注入的实现方式
public class Notification {
  private MessageSender messageSender;
  
  // 通过构造函数将messageSender传递进来
  public Notification(MessageSender messageSender) {
    this.messageSender = messageSender;
  }
  
  public void sendMessage(String cellphone, String message) {
    //...省略校验逻辑等...
    this.messageSender.send(cellphone, message);
  }
}
//使用Notification
MessageSender messageSender = new MessageSender();
Notification notification = new Notification(messageSender);
```