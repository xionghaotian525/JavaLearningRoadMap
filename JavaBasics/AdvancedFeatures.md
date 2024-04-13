# AdvancedFeatures

## 目录
[toc]
## 一、个人总结
### 1.1 Java集合框架
![](/Res/images/集合接口继承关系和实现.png)
#### 1.1.1 Collection
1. **List**

    1. **ArrayList**

- 概述
![](/Res/images/集合框架-Collection-List-ArrayList.png)
  - `ArrayList`实现了`List`接口，是顺序容器，即元素存放的数据与放进去的顺序相同，允许放入`null`元素，**底层通过数组实现**。除该类**未实现同步**外，其余跟Vector大致相同。每个ArrayList都有一个容量(capacity)，表示底层数组的实际大小，容器内存储元素的个数不能多于当前容量。当向容器中添加元素时，如果容量不足，容器会**自动增大底层数组的大小**。前面已经提过，Java泛型只是编译器提供的语法糖，所以这里的数组是一个Object数组，以便能够容纳任何类型的对象。
  - `size()`, `isEmpty()`, `get()`, `set()`方法均能在常数时间内完成，`add()`方法的时间开销跟插入位置有关，`addAll()`方法的时间开销跟添加元素的个数成正比。其余方法大都是线性时间。
  - 为追求效率，ArrayList没有实现同步(`synchronized`)，如果需要多个线程并发访问，用户可以手动同步，也可使用Vector替代

- **自动扩容**
`add(E e)、addAll(int index, E e)`这两个方法都是向容器中添加新元素，这可能会导致`capacity`不足，因此在添加元素之前，都需要进行**剩余空间检查**，如果需要则自动扩容。扩容操作最终是通过`grow()`方法完成的
![](/Res/images/集合框架-Collection-List-ArrayList-自动扩容.png)
  - 每当向数组中添加元素时，都要去检查添加后元素的个数是否会超出当前数组的长度，如果超出，数组将会进行扩容，以满足添加数据的需求。数组扩容通过一个公开的方法`ensureCapacity(int minCapacity)`来实现。在实际添加大量元素前，我也可以使用`ensureCapacity`来手动增加ArrayList实例的容量，以减少递增式再分配的数量。
  - 数组进行扩容时，会将老数组中的元素重新拷贝一份到新的数组中，每次数组容量的增长大约是其原容量的`1.5`倍。这种操作的代价是很高的，因此在实际使用时，我们应该尽量避免数组容量的扩张。当我们可预知要保存的元素的多少时，要在构造ArrayList实例时，就指定其容量，以避免数组扩容的发生。或者根据实际需求，通过调用`ensureCapacity`方法来手动增加ArrayList实例的容量

- add()、addAll()
![](/Res/images/集合框架-Collection-List-ArrayList-add().png)
```java
/**
 * Appends the specified element to the end of this list.
 *
 * @param e element to be appended to this list
 * @return <tt>true</tt> (as specified by {@link Collection#add})
 */
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
}

/**
 * Inserts the specified element at the specified position in this
 * list. Shifts the element currently at that position (if any) and
 * any subsequent elements to the right (adds one to their indices).
 *
 * @param index index at which the specified element is to be inserted
 * @param element element to be inserted
 * @throws IndexOutOfBoundsException {@inheritDoc}
 */
public void add(int index, E element) {
    rangeCheckForAdd(index);

    ensureCapacityInternal(size + 1);  // Increments modCount!!
    System.arraycopy(elementData, index, elementData, index + 1,
                        size - index);
    elementData[index] = element;
    size++;
}
```
`addAll()`方法能够一次添加多个元素，根据位置不同也有两个版本，一个是在末尾添加的`addAll(Collection<? extends E> c)`方法，一个是从指定位置开始插入的`addAll(int index, Collection<? extends E> c)`方法。跟`add()`方法类似，在插入之前也需要进行**空间检查**，如果需要则自动扩容；如果从指定位置插入，也会存在移动元素的情况。 `addAll()`的时间复杂度不仅跟插入元素的多少有关，也跟插入的位置相关。
```java
/**
 * Appends all of the elements in the specified collection to the end of
 * this list, in the order that they are returned by the
 * specified collection's Iterator.  The behavior of this operation is
 * undefined if the specified collection is modified while the operation
 * is in progress.  (This implies that the behavior of this call is
 * undefined if the specified collection is this list, and this
 * list is nonempty.)
 *
 * @param c collection containing elements to be added to this list
 * @return <tt>true</tt> if this list changed as a result of the call
 * @throws NullPointerException if the specified collection is null
 */
public boolean addAll(Collection<? extends E> c) {
    Object[] a = c.toArray();
    int numNew = a.length;
    ensureCapacityInternal(size + numNew);  // Increments modCount
    System.arraycopy(a, 0, elementData, size, numNew);
    size += numNew;
    return numNew != 0;
}

/**
 * Inserts all of the elements in the specified collection into this
 * list, starting at the specified position.  Shifts the element
 * currently at that position (if any) and any subsequent elements to
 * the right (increases their indices).  The new elements will appear
 * in the list in the order that they are returned by the
 * specified collection's iterator.
 *
 * @param index index at which to insert the first element from the
 *              specified collection
 * @param c collection containing elements to be added to this list
 * @return <tt>true</tt> if this list changed as a result of the call
 * @throws IndexOutOfBoundsException {@inheritDoc}
 * @throws NullPointerException if the specified collection is null
 */
public boolean addAll(int index, Collection<? extends E> c) {
    rangeCheckForAdd(index);

    Object[] a = c.toArray();
    int numNew = a.length;
    ensureCapacityInternal(size + numNew);  // Increments modCount

    int numMoved = size - index;
    if (numMoved > 0)
        System.arraycopy(elementData, index, elementData, index + numNew,
                            numMoved);

    System.arraycopy(a, 0, elementData, index, numNew);
    size += numNew;
    return numNew != 0;
}
```
- set()
```java
public E set(int index, E element) {
    rangeCheck(index);//下标越界检查
    E oldValue = elementData(index);
    elementData[index] = element;//赋值到指定位置，复制的仅仅是引用
    return oldValue;
}
```
- get()
```java
public E get(int index) {
    rangeCheck(index);
    return (E) elementData[index];//注意类型转换
}
```
- remove()
`remove()`方法也有两个版本，一个是`remove(int index)`删除指定位置的元素，另一个是`remove(Object o)`删除第一个满足`o.equals(elementData[index])`的元素。删除操作是`add()`操作的逆过程，需要将删除点之后的元素向前移动一个位置。需要注意的是为了让GC起作用，必须显式的为最后一个位置赋null值。
```java
public E remove(int index) {
    rangeCheck(index);
    modCount++;
    E oldValue = elementData(index);
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index, numMoved);
    elementData[--size] = null; //清除该位置的引用，让GC起作用
    return oldValue;
}
```
关于Java GC这里需要特别说明一下，有了垃圾收集器并不意味着一定不会有内存泄漏。对象能否被GC的依据是是否还有引用指向它，上面代码中如果不手动赋null值，除非对应的位置被其他元素覆盖，否则原来的对象就一直不会被回收。
- trimToSize()
ArrayList还给我们提供了**将底层数组的容量调整为当前列表保存的实际元素的大小**的功能。它可以通过trimToSize方法来实现。代码如下:
```java
/**
 * Trims the capacity of this <tt>ArrayList</tt> instance to be the
 * list's current size.  An application can use this operation to minimize
 * the storage of an <tt>ArrayList</tt> instance.
 */
public void trimToSize() {
    modCount++;
    if (size < elementData.length) {
        elementData = (size == 0)
            ? EMPTY_ELEMENTDATA
            : Arrays.copyOf(elementData, size);
    }
}
```
- indexOf()、lastIndexOf()
获取元素的第一次出现的index:
```java
/**
 * Returns the index of the first occurrence of the specified element
 * in this list, or -1 if this list does not contain the element.
 * More formally, returns the lowest index <tt>i</tt> such that
 * <tt>(o==null&nbsp;?&nbsp;get(i)==null&nbsp;:&nbsp;o.equals(get(i)))</tt>,
 * or -1 if there is no such index.
 */
public int indexOf(Object o) {
    if (o == null) {
        for (int i = 0; i < size; i++)
            if (elementData[i]==null)
                return i;
    } else {
        for (int i = 0; i < size; i++)
            if (o.equals(elementData[i]))
                return i;
    }
    return -1;
}
```
获取元素的最后一次出现的index:
```java
 /**
 * Returns the index of the last occurrence of the specified element
 * in this list, or -1 if this list does not contain the element.
 * More formally, returns the highest index <tt>i</tt> such that
 * <tt>(o==null&nbsp;?&nbsp;get(i)==null&nbsp;:&nbsp;o.equals(get(i)))</tt>,
 * or -1 if there is no such index.
 */
public int lastIndexOf(Object o) {
    if (o == null) {
        for (int i = size-1; i >= 0; i--)
            if (elementData[i]==null)
                return i;
    } else {
        for (int i = size-1; i >= 0; i--)
            if (o.equals(elementData[i]))
                return i;
    }
    return -1;
}
```
- Fail-Fast机制
ArrayList也采用了快速失败的机制，通过记录`modCount`参数来实现。在面对并发的修改时，迭代器很快就会完全失败，而不是冒着在将来某个不确定时间发生任意不确定行为的风险。

    1. **Vector**

- 1

    3. **LinkedList**

- 概述
![](/Res/images/集合框架-Collection-List-LinkedList.png)
LinkedList底层通过**双向链表**实现。LinkedList同时实现了`List`接口和`Deque`接口，也就是说它既可以看作一个顺序容器，又可以看作一个队列(Queue)，同时又可以看作一个栈(Stack)。这样看来，LinkedList简直就是个全能冠军。当你需要使用栈或者队列时，可以考虑使用LinkedList，一方面是因为Java官方已经声明不建议使用Stack类，更遗憾的是，Java里根本没有一个叫做Queue的类(它是个接口名字)。关于栈或队列，现在的首选是`ArrayDeque`，它有着比LinkedList(当作栈或队列使用时)有着更好的性能

- getFirst(), getLast()
- removeFirst(), removeLast(), remove(e), remove(index)
- add()
- addAll()
- clear()
- Positional Access 方法
- 查找操作
- Queue 方法
- Deque 方法


1. **Set**
2. **Queue**


#### 1.1.2 Iterator

#### 1.1.3 Map
1. **HashMap**
2. **TreeMap**
3. **HashTable**
4. **LinkedHashMap**

### 1.2 输入输出
![](/Res/images/JavaIO包.png)
![](/Res/images/JavaNIO包.png)
### 1.3 异常处理
![](/Res/images/)
### 1.4 反射
### 1.5 泛型
### 1.6 序列化
### 1.7 注解
### 1.8 lambda表达式
### 1.9 字符串处理
### 1.10 网络编程

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
