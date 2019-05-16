# effective-java

## 第1章	引言

## 第2章	创建和销毁对象

第1条：用静态工厂方法代替构造器  

第2条：遇到多个构造器参数时要考虑使用构建器  

第3条：用私有构造器或者枚举类型强化Singleton属性  

第4条：通过私有构造器强化不可实例化的能力  

第5条：优先考虑依赖注入来引用资源  

第6条：避免创建不必要的对象  

第7条：消除过期的对象引用  

第8条：避免使用终结方法和清除方法  

第9条：try-with-resources优先于try-finally  

## 第3章	对于所有对象都通用的方法

第10条：覆盖equals时请遵守通用约定  

第11条：覆盖equals时总要覆盖hashCode  

第12条：始终要覆盖toString  

第13条：谨慎地覆盖clone  

第14条：考虑实现Comparable接口 

## 第4章	类和接口

第15条：使类和成员的可访问性最小化  

第16条：要在公有类而非公有域中使用访问方法  

第17条：使可变性最小化  

第18条：复合优先于继承  

第19条：要么设计继承并提供文档说明，要么禁止继承  

第20条：接口优于抽象类  

第21条：为后代设计接口  

第22条：接口只用于定义类型  

第23条：类层次优于标签类  

第24条：静态成员类优于非静态成员类  

第25条：限制源文件为单个顶级类  

## 第5章	泛型

第26条：请不要使用原生态类型  

`List`相对`List<String>`是raw type(即不带参数的泛型)，使用raw type使泛型的类型信息会无法从声明中体现。使用raw type，可以存放不同类型的数据。

`List<String>`是`List`的子类型，但是不是`List<Object>`的子类型

有限制通配符List<? extend T>, List<? super T>和无限制通配符

Set<Object>是带参数泛型可以包含任意类型的对象，而Set<?>是通配类型表示一个可以存储某一类型对象的集合。



  

| Term                    | Example                          | Item   |
| ----------------------- | -------------------------------- | ------ |
| Parameterized type      | List<String>                     | 26     |
| Actual type             | String                           | 26     |
| parameter Generic type  | List<E>                          | 26，29 |
| Formal type parameter   | E                                | 26     |
| Unbounded wildcard type | List<?>                          | 26     |
| Raw type                | List                             | 26     |
| Bounded type parameter  | <E extends Number>               | 29     |
| Recursive type bound    | <T extends Comparable<T>>        | 30     |
| Bounded wildcard type   | List<? extends Number>           | 31     |
| Generic method          | static <E> List<E> asList(E[] a) | 30     |
| type token              | String.class                     | 33     |

​	

第27条：消除非受检的警告  

@SuppressWarnings("unchecked")

第28条：列表优于数组  

covariant 协变，Sub是Super的子类，那么Sub[]也是Super[]的子类，与之相对的名次是invariant，List<Type1>和List<Type2>没有与之类似的关系。

reified:array在runtime才限制元素的类型。

nonreifiable:在运行时包含的信息比编译时的信息更少。

可变参数方法，本来就是要把不同类型的参数放在一个list里，这时就需要增加SafeVarargs的注解。

泛型数据容易导致ClassCastException

第29条：优先考虑泛型  

第30条：优先考虑泛型方法  

第31条：利用有限制通配符来提升API的灵活性 

第32条：谨慎并用泛型和可变参数  

第33条：优先考虑类型安全的异构容器  

## 第6章	枚举和注解

第34条：用enum代替int常量  

第35条：用实例域代替序数  

第36条：用EnumSet代替位域  

第37条：用EnumMap代替序数索引  

第38条：用接口模拟可扩展的枚举  

第39条：注解优先于命名模式  

第40条：坚持使用Override注解 

第41条：用标记接口定义类型  

## 第7章	Lambda和Stream

第42条：Lambda优先于匿名类  

只有一个抽象方法的函数接口（function interface），实现该抽象方法的实例为lambda expression，简称为lambda，作用和匿名类相似且更为简洁。

```java
// Anonymous class instance as a function object - obsolete!
Collections.sort(words, new Comparator<String>() {
    public int compare(String s1, String s2) {
        return Integer.compare(s1.length(), s2.length());
    }
} );
```



```java
//Lambda expression as function object(replace anonymous class)
Collections.sort(words,
                (s1, s2) -> Integer.compare(s1.length(), s2.length()));
```

匿名类应用的三个场景：

1. 需要实例化抽象类
2. 需要实现好几个抽象方法
3. This的指向不同，在lambda函数中，this指向包含它的实例，而匿名类指向自己

第43条：方法引用优先于Lambda  

第44条：坚持使用标准的函数接口  

第45条：谨慎使用Stream  

第46条：优先选择Stream中无副作用的函数  

第47条：Stream要优先用Collection作为返回类型  

第48条：谨慎使用Stream并行 

## 第8章	方法

第49条：检查参数的有效性  

第50条：必要时进行保护性拷贝  

第51条：谨慎设计方法签名  

第52条：慎用重载  

第53条：慎用可变参数  

第54条：返回零长度的数组或者集合，而不是null  

第55条：谨慎返回optinal  

第56条：为所有导出的API元素编写文档注释  

## 第9章	通用编程

第57条：将局部变量的作用域最小化  

第58条：for-each循环优先于传统的for循环 

第59条：了解和使用类库  

第60条：如果需要精确的答案，请避免使用float和double  

第61条：基本类型优先于装箱基本类型  

第62条：如果其他类型更适合，则尽量避免使用字符串  

第63条：了解字符串连接的性能  

第64条：通过接口引用对象  

第65条：接口优先于反射机制  

第66条：谨慎地使用本地方法  

第67条：谨慎地进行优化  

第68条：遵守普遍接受的命名惯例  

## 第10章	异常

第69条：只针对异常的情况才使用异常  

第70条：对可恢复的情况使用受检异常，对编程错误使用运行时异常  

第71条：避免不必要地使用受检异常  

第72条：优先使用标准的异常  

第73条：抛出与抽象对应的异常  

第74条：每个方法抛出的所有异常都要建立文档  

第75条：在细节消息中包含失败-捕获信息 

第76条：努力使失败保持原子性  

第77条：不要忽略异常  

## 第11章	并发

第78条：同步访问共享的可变数据  

第79条：避免过度同步  

第80条：executor、task和stream优先于线程  

第81条：并发工具优先于wait和notify  

第82条：线程安全性的文档化  

第83条：慎用延迟初始化  

第84条：不要依赖于线程调度器  

## 第12章	序列化

第85条：其他方法优先于Java序列化 

第86条：谨慎地实现Serializable接口 

第87条：考虑使用自定义的序列化形式  

第88条：保护性地编写readObject方法 

第89条：对于实例控制，枚举类型优先于readResolve  

第90条：考虑用序列化代理代替序列化实例  

## 附录　与第2版中条目的对应关系  286

## 参考文献  289





