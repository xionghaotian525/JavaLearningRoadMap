# AdvancedFeatures

## 目录
[toc]
## 一、个人总结
### Java集合框架
![](/Res/images/集合接口继承关系和实现.png)
### 输入输出
![](/Res/images/JavaIO包.png)
![](/Res/images/JavaNIO包.png)
### 异常处理
![](/Res/images/)
### 反射
### 泛型
### 序列化
### 注解
### lambda表达式
### 字符串处理
### 网络编程

## 二、八股问题整理(高级特性与基础部分其实都是基础部分，有混杂的问题，后序待修订使条理更清晰)
### 集合
### 反射
### 泛型
### IO流
### 异常处理
### 注解
### lambda表达式
### 字符串处理
### 网络编程
#### (10)String、StringBuffer、StringBuilder的区别

#### (11)ArrayList、Vector和linkedList的区别

#### (12)HashMap和HashTable的区别

#### (13)Collection包结构，与Collections的区别

#### (15)泛型常用特点

#### (23)try catch finally，try里有return，finally还执行么？

#### (24)Excption与Error包结构

#### (25)OOM你遇到过哪些情况，SOF你遇到过哪些情况

#### (27)Java 序列化中如果有些字段不想进行序列化，怎么办？

#### (28)说说Java 中 IO 流

#### (29)Java IO与 NIO的区别（补充）

#### (30)java反射的作用与原理

#### (31)说说List,Set,Map三者的区别？

#### (33)获取一个类Class对象的方式有哪些？(java反射部分30)

#### (35)说说什么是 fail-fast？

#### (36)HashMap 中的 key 我们可以使用任何类作为 key 吗？

#### (37)HashMap 的长度为什么是 2 的 N 次方呢？

#### (38)HashMap 与 ConcurrentHashMap 的异同

#### (39)红黑树的特征

#### (40)说说你平时是怎么处理 Java 异常的

#### (47)出现在Java程序中的finally代码块是否一定会执行？

#### (49)序列化是什么？

#### (50)简述throw与throws的区别

#### (51)简述Java序列化与反序列化的实现

#### (52)Java中线程安全的基本数据结构有哪些

#### (53)fail-fast和fail-safe迭代器的区别是什么？

#### (54)HashMap底层

#### (55)concurrentHashMap是如何实现线程安全的(put 如下；get volatile)

#### (41).Java 中，Serializable 与 Externalizable 的区别?
Serializable 接口是一个序列化 Java 类的接口，以便于它们可以在网络上传输或者可以将它们的状态保存在磁盘上，是 JVM 内嵌的默认序列化方式，成本高、脆弱而且不安全。Externalizable 允许你控制整个序列化过程，指定特定的二进制格式，增加安全机制
> from pdai

泛型：
为什么需要泛型？
泛型类如何定义使用？
泛型接口如何定义使用？
泛型方法如何定义使用？
泛型的上限和下限？
如何理解Java中的泛型是伪泛型？
注解：
注解的作用？
注解的常见分类？
1.4 异常
Java异常类层次结构?
可查的异常（checked exceptions）和不可查的异常（unchecked exceptions）区别？
throw和throws的区别？
Java 7 的 try-with-resource?
异常的底层？
1.5 反射什么是反射？
反射的使用？
getName、getCanonicalName与getSimpleName的区别?
1.6 SPI机制
什么是SPI机制？
SPI机制的应用？
SPI机制的简单示例？
2 Java 集合
2.1 Collection
集合有哪些类？
ArrayList的底层？
ArrayList自动扩容？
ArrayList的Fail-Fast机制？
2.2 Map
Map有哪些类？
JDK7 HashMap如何实现？
JDK8 HashMap如何实现？
HashSet是如何实现的？
什么是WeakHashMap?
