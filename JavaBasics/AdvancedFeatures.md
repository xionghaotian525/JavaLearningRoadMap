# AdvancedFeatures

## 目录
[toc]
## 一、个人总结
### 1.1 Java集合框架
![](/Res/images/集合接口继承关系和实现.png)

#### 1.1.1 Collection
![](/Res/images/集合框架-概述.png)
1. **List**

    1. **==ArrayList==**
![](/Res/images/集合框架-Collection-List-ArrayList层次结构图.png)
- 概述
![](/Res/images/集合框架-Collection-List-ArrayList.png)
  - `ArrayList`实现了`List`接口，是顺序容器，即元素存放的数据与放进去的顺序相同，允许放入`null`元素，**底层通过数组实现**。除该类**未实现同步**外，其余跟Vector大致相同。每个ArrayList都有一个容量(capacity)，表示底层数组的实际大小，容器内存储元素的个数不能多于当前容量。当向容器中添加元素时，如果容量不足，容器会**自动增大底层数组的大小**。前面已经提过，Java泛型只是编译器提供的语法糖，所以这里的数组是一个Object数组，以便能够容纳任何类型的对象。
  - `size()`, `isEmpty()`, `get()`, `set()`方法均能在常数时间内完成，`add()`方法的时间开销跟插入位置有关，`addAll()`方法的时间开销跟添加元素的个数成正比。其余方法大都是线性时间。
  - 为追求效率，ArrayList没有实现同步(`synchronized`)，如果需要多个线程并发访问，用户可以手动同步，也可使用Vector替代
- 底层数据结构
```java
/**
 * The array buffer into which the elements of the ArrayList are stored.
 * The capacity of the ArrayList is the length of this array buffer. Any
 * empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
 * will be expanded to DEFAULT_CAPACITY when the first element is added.
 */
transient Object[] elementData; // non-private to simplify nested class access

/**
 * The size of the ArrayList (the number of elements it contains).
 *
 * @serial
 */
private int size;
```
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

    2. **==Vector==**
![](/Res/images/集合框架-Collection-List-Vector层次结构图.png)
- 概述
Vector类是单列集合List接口的一个实现类。与ArrayList类似，Vector也实现了一个可以动态修改的数组，两者最本质的区别在于——**Vector类是支持线程同步的**，因此它线程安全，支持多线程；而ArrayList是线程不同步的，线程不安全。Vector**底层也是由一个Object类型的数组**来实现的 (注意Vector维护的elementData数组没有用`transient`关键字修饰)
- 底层数据结构
```java
/**
* The internal array used to hold members of a Vector. The elements are
* in positions 0 through elementCount - 1, and all remaining slots are null.
* @serial the elements
*/
protected T[] elementData;

/**
* The number of elements currently in the vector, also returned by
* {@link #size}.
* @serial the size
*/
protected int elementCount;

/**
* The amount the Vector's internal array should be increased in size when
* a new element is added that exceeds the current size of the array,
* or when {@link #ensureCapacity} is called. If &lt;= 0, the vector just
* doubles in size.
* @serial the amount to grow the vector by
*/
protected int capacityIncrement;  
```
- 常用方法略，和ArrayList很像
- Synchronization - All methods are **synchronized**, making it thread-safe.
- **capacity()** - Returns the current capacity of the Vector.
- **addElement(E obj)** - Adds an element to the end of the Vector.
- Doubles its array size whenever it's resized.

    3. **==LinkedList==**
![](/Res/images/集合框架-Collection-List-LinkedList层次结构图.png)
- 概述
![](/Res/images/集合框架-Collection-List-LinkedList.png)
  - LinkedList底层通过 **双向链表(Deque)** 实现。LinkedList同时实现了`List`接口和`Deque`接口，也就是说它既可以看作一个顺序容器，又可以看作一个队列(Queue)，同时又可以看作一个栈(Stack)。这样看来，LinkedList简直就是个全能冠军。当你需要使用栈或者队列时，可以考虑使用LinkedList，一方面是因为Java官方已经声明不建议使用Stack类，更遗憾的是，Java里根本没有一个叫做Queue的类(它是个接口名字)。关于栈或队列，现在的首选是`ArrayDeque`，它有着比LinkedList(当作栈或队列使用时)有着更好的性能
  - LinkedList的实现方式决定了所有跟下标相关的操作都是线性时间，而在首段或者末尾删除元素只需要常数时间。为追求效率LinkedList没有实现同步(`synchronized`)，如果需要多个线程并发访问，可以先采用`Collections.synchronizedList()`方法对其进行包装。

- getFirst(), getLast()

获取第一个元素， 和获取最后一个元素:
```java
/**
 * Returns the first element in this list.
 *
 * @return the first element in this list
 * @throws NoSuchElementException if this list is empty
 */
public E getFirst() {
    final Node<E> f = first;
    if (f == null)
        throw new NoSuchElementException();
    return f.item;
}

/**
 * Returns the last element in this list.
 *
 * @return the last element in this list
 * @throws NoSuchElementException if this list is empty
 */
public E getLast() {
    final Node<E> l = last;
    if (l == null)
        throw new NoSuchElementException();
    return l.item;
}
```
- removeFirst(), removeLast(), remove(e), remove(index)

`remove()`方法也有两个版本，一个是删除跟指定元素相等的第一个元素`remove(Object o)`，另一个是删除指定下标处的元素`remove(int index)`。
![](/Res/images/集合框架-Collection-List-LinkedList-remove().png)
删除元素 - 指的是删除第一次出现的这个元素, 如果没有这个元素，则返回false；判断的依据是equals方法， 如果equals，则直接unlink这个node；由于LinkedList可存放null元素，故也可以删除第一次出现null的元素
```java
/**
 * Removes the first occurrence of the specified element from this list,
 * if it is present.  If this list does not contain the element, it is
 * unchanged.  More formally, removes the element with the lowest index
 * {@code i} such that
 * <tt>(o==null&nbsp;?&nbsp;get(i)==null&nbsp;:&nbsp;o.equals(get(i)))</tt>
 * (if such an element exists).  Returns {@code true} if this list
 * contained the specified element (or equivalently, if this list
 * changed as a result of the call).
 *
 * @param o element to be removed from this list, if present
 * @return {@code true} if this list contained the specified element
 */
public boolean remove(Object o) {
    if (o == null) {
        for (Node<E> x = first; x != null; x = x.next) {
            if (x.item == null) {
                unlink(x);
                return true;
            }
        }
    } else {
        for (Node<E> x = first; x != null; x = x.next) {
            if (o.equals(x.item)) {
                unlink(x);
                return true;
            }
        }
    }
    return false;
}

/**
 * Unlinks non-null node x.
 */
E unlink(Node<E> x) {
    // assert x != null;
    final E element = x.item;
    final Node<E> next = x.next;
    final Node<E> prev = x.prev;

    if (prev == null) {// 第一个元素
        first = next;
    } else {
        prev.next = next;
        x.prev = null;
    }

    if (next == null) {// 最后一个元素
        last = prev;
    } else {
        next.prev = prev;
        x.next = null;
    }

    x.item = null; // GC
    size--;
    modCount++;
    return element;
}
```
`remove(int index)`使用的是下标计数， 只需要判断该index是否有元素即可，如果有则直接unlink这个node。
```java
/**
 * Removes the element at the specified position in this list.  Shifts any
 * subsequent elements to the left (subtracts one from their indices).
 * Returns the element that was removed from the list.
 *
 * @param index the index of the element to be removed
 * @return the element previously at the specified position
 * @throws IndexOutOfBoundsException {@inheritDoc}
 */
public E remove(int index) {
    checkElementIndex(index);
    return unlink(node(index));
}
```
删除head元素:
```java
/**
 * Removes and returns the first element from this list.
 *
 * @return the first element from this list
 * @throws NoSuchElementException if this list is empty
 */
public E removeFirst() {
    final Node<E> f = first;
    if (f == null)
        throw new NoSuchElementException();
    return unlinkFirst(f);
}
/**
 * Unlinks non-null first node f.
 */
private E unlinkFirst(Node<E> f) {
    // assert f == first && f != null;
    final E element = f.item;
    final Node<E> next = f.next;
    f.item = null;
    f.next = null; // help GC
    first = next;
    if (next == null)
        last = null;
    else
        next.prev = null;
    size--;
    modCount++;
    return element;
}
```
删除last元素:
```java
/**
 * Removes and returns the last element from this list.
 *
 * @return the last element from this list
 * @throws NoSuchElementException if this list is empty
 */
public E removeLast() {
    final Node<E> l = last;
    if (l == null)
        throw new NoSuchElementException();
    return unlinkLast(l);
}
/**
 * Unlinks non-null last node l.
 */
private E unlinkLast(Node<E> l) {
    // assert l == last && l != null;
    final E element = l.item;
    final Node<E> prev = l.prev;
    l.item = null;
    l.prev = null; // help GC
    last = prev;
    if (prev == null)
        first = null;
    else
        prev.next = null;
    size--;
    modCount++;
    return element;
}
```

- add()

`add()`方法有两个版本，一个是`add(E e)`，该方法在LinkedList的末尾插入元素，因为有last指向链表末尾，在末尾插入元素的花费是常数时间。只需要简单修改几个相关引用即可；另一个是`add(int index, E element)`，该方法是在指定下表处插入元素，需要先通过线性查找找到具体位置，然后修改相关引用完成插入操作。
```java
/**
 * Appends the specified element to the end of this list.
 *
 * <p>This method is equivalent to {@link #addLast}.
 *
 * @param e element to be appended to this list
 * @return {@code true} (as specified by {@link Collection#add})
 */
public boolean add(E e) {
    linkLast(e);
    return true;
}

/**
 * Links e as last element.
 */
void linkLast(E e) {
    final Node<E> l = last;
    final Node<E> newNode = new Node<>(l, e, null);
    last = newNode;
    if (l == null)
        first = newNode;
    else
        l.next = newNode;
    size++;
    modCount++;
}
```
![](/Res/images/集合框架-Collection-List-LinkedList-add().png)
`add(int index, E element)`, 当`index==size`时，等同于`add(E e)`; 如果不是，则分两步: 1.先根据index找到要插入的位置,即`node(index)`方法；2.修改引用，完成插入操作。
```java
/**
 * Inserts the specified element at the specified position in this list.
 * Shifts the element currently at that position (if any) and any
 * subsequent elements to the right (adds one to their indices).
 *
 * @param index index at which the specified element is to be inserted
 * @param element element to be inserted
 * @throws IndexOutOfBoundsException {@inheritDoc}
 */
public void add(int index, E element) {
    checkPositionIndex(index);

    if (index == size)
        linkLast(element);
    else
        linkBefore(element, node(index));
}
```
上面代码中的`node(int index)`函数有一点小小的trick，因为链表双向的，可以从开始往后找，也可以从结尾往前找，具体朝那个方向找取决于条件`index < (size >> 1)`，也即是index是靠近前端还是后端。从这里也可以看出，**linkedList通过index检索元素的效率没有arrayList高。**
```java
/**
 * Returns the (non-null) Node at the specified element index.
 */
Node<E> node(int index) {
    // assert isElementIndex(index);

    if (index < (size >> 1)) {
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```
- addAll()

addAll(index, c) 实现方式并不是直接调用add(index,e)来实现，主要是因为效率的问题，另一个是fail-fast中modCount只会增加1次；
```java
/**
 * Appends all of the elements in the specified collection to the end of
 * this list, in the order that they are returned by the specified
 * collection's iterator.  The behavior of this operation is undefined if
 * the specified collection is modified while the operation is in
 * progress.  (Note that this will occur if the specified collection is
 * this list, and it's nonempty.)
 *
 * @param c collection containing elements to be added to this list
 * @return {@code true} if this list changed as a result of the call
 * @throws NullPointerException if the specified collection is null
 */
public boolean addAll(Collection<? extends E> c) {
    return addAll(size, c);
}

/**
 * Inserts all of the elements in the specified collection into this
 * list, starting at the specified position.  Shifts the element
 * currently at that position (if any) and any subsequent elements to
 * the right (increases their indices).  The new elements will appear
 * in the list in the order that they are returned by the
 * specified collection's iterator.
 *
 * @param index index at which to insert the first element
 *              from the specified collection
 * @param c collection containing elements to be added to this list
 * @return {@code true} if this list changed as a result of the call
 * @throws IndexOutOfBoundsException {@inheritDoc}
 * @throws NullPointerException if the specified collection is null
 */
public boolean addAll(int index, Collection<? extends E> c) {
    checkPositionIndex(index);

    Object[] a = c.toArray();
    int numNew = a.length;
    if (numNew == 0)
        return false;

    Node<E> pred, succ;
    if (index == size) {
        succ = null;
        pred = last;
    } else {
        succ = node(index);
        pred = succ.prev;
    }

    for (Object o : a) {
        @SuppressWarnings("unchecked") E e = (E) o;
        Node<E> newNode = new Node<>(pred, e, null);
        if (pred == null)
            first = newNode;
        else
            pred.next = newNode;
        pred = newNode;
    }

    if (succ == null) {
        last = pred;
    } else {
        pred.next = succ;
        succ.prev = pred;
    }

    size += numNew;
    modCount++;
    return true;
}
```
- clear()

为了让GC更快可以回收放置的元素，需要将node之间的引用关系赋空。
```java
/**
 * Removes all of the elements from this list.
 * The list will be empty after this call returns.
 */
public void clear() {
    // Clearing all of the links between nodes is "unnecessary", but:
    // - helps a generational GC if the discarded nodes inhabit
    //   more than one generation
    // - is sure to free memory even if there is a reachable Iterator
    for (Node<E> x = first; x != null; ) {
        Node<E> next = x.next;
        x.item = null;
        x.next = null;
        x.prev = null;
        x = next;
    }
    first = last = null;
    size = 0;
    modCount++;
}
```
- Positional Access 方法

通过index获取元素
```java
/**
 * Returns the element at the specified position in this list.
 *
 * @param index index of the element to return
 * @return the element at the specified position in this list
 * @throws IndexOutOfBoundsException {@inheritDoc}
 */
public E get(int index) {
    checkElementIndex(index);
    return node(index).item;
}
```
将某个位置的元素重新赋值:
```java
/**
 * Replaces the element at the specified position in this list with the
 * specified element.
 *
 * @param index index of the element to replace
 * @param element element to be stored at the specified position
 * @return the element previously at the specified position
 * @throws IndexOutOfBoundsException {@inheritDoc}
 */
public E set(int index, E element) {
    checkElementIndex(index);
    Node<E> x = node(index);
    E oldVal = x.item;
    x.item = element;
    return oldVal;
}
```
将元素插入到指定index位置:
```java
/**
 * Inserts the specified element at the specified position in this list.
 * Shifts the element currently at that position (if any) and any
 * subsequent elements to the right (adds one to their indices).
 *
 * @param index index at which the specified element is to be inserted
 * @param element element to be inserted
 * @throws IndexOutOfBoundsException {@inheritDoc}
 */
public void add(int index, E element) {
    checkPositionIndex(index);

    if (index == size)
        linkLast(element);
    else
        linkBefore(element, node(index));
}
```
删除指定位置的元素:
```java
    /**
     * Removes the element at the specified position in this list.  Shifts any
     * subsequent elements to the left (subtracts one from their indices).
     * Returns the element that was removed from the list.
     *
     * @param index the index of the element to be removed
     * @return the element previously at the specified position
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public E remove(int index) {
        checkElementIndex(index);
        return unlink(node(index));
    }
```
其它位置的方法:
```java
/**
 * Tells if the argument is the index of an existing element.
 */
private boolean isElementIndex(int index) {
    return index >= 0 && index < size;
}

/**
 * Tells if the argument is the index of a valid position for an
 * iterator or an add operation.
 */
private boolean isPositionIndex(int index) {
    return index >= 0 && index <= size;
}

/**
 * Constructs an IndexOutOfBoundsException detail message.
 * Of the many possible refactorings of the error handling code,
 * this "outlining" performs best with both server and client VMs.
 */
private String outOfBoundsMsg(int index) {
    return "Index: "+index+", Size: "+size;
}

private void checkElementIndex(int index) {
    if (!isElementIndex(index))
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}

private void checkPositionIndex(int index) {
    if (!isPositionIndex(index))
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}
```
- 查找操作

查找操作的本质是查找元素的下标:

查找第一次出现的index, 如果找不到返回-1；
```java
/**
 * Returns the index of the first occurrence of the specified element
 * in this list, or -1 if this list does not contain the element.
 * More formally, returns the lowest index {@code i} such that
 * <tt>(o==null&nbsp;?&nbsp;get(i)==null&nbsp;:&nbsp;o.equals(get(i)))</tt>,
 * or -1 if there is no such index.
 *
 * @param o element to search for
 * @return the index of the first occurrence of the specified element in
 *         this list, or -1 if this list does not contain the element
 */
public int indexOf(Object o) {
    int index = 0;
    if (o == null) {
        for (Node<E> x = first; x != null; x = x.next) {
            if (x.item == null)
                return index;
            index++;
        }
    } else {
        for (Node<E> x = first; x != null; x = x.next) {
            if (o.equals(x.item))
                return index;
            index++;
        }
    }
    return -1;
}
```
查找最后一次出现的index, 如果找不到返回-1
```java
/**
 * Returns the index of the last occurrence of the specified element
 * in this list, or -1 if this list does not contain the element.
 * More formally, returns the highest index {@code i} such that
 * <tt>(o==null&nbsp;?&nbsp;get(i)==null&nbsp;:&nbsp;o.equals(get(i)))</tt>,
 * or -1 if there is no such index.
 *
 * @param o element to search for
 * @return the index of the last occurrence of the specified element in
 *         this list, or -1 if this list does not contain the element
 */
public int lastIndexOf(Object o) {
    int index = size;
    if (o == null) {
        for (Node<E> x = last; x != null; x = x.prev) {
            index--;
            if (x.item == null)
                return index;
        }
    } else {
        for (Node<E> x = last; x != null; x = x.prev) {
            index--;
            if (o.equals(x.item))
                return index;
        }
    }
    return -1;
}
```
- Queue 方法

```java
/**
 * Retrieves, but does not remove, the head (first element) of this list.
 *
 * @return the head of this list, or {@code null} if this list is empty
 * @since 1.5
 */
public E peek() {
    final Node<E> f = first;
    return (f == null) ? null : f.item;
}

/**
 * Retrieves, but does not remove, the head (first element) of this list.
 *
 * @return the head of this list
 * @throws NoSuchElementException if this list is empty
 * @since 1.5
 */
public E element() {
    return getFirst();
}

/**
 * Retrieves and removes the head (first element) of this list.
 *
 * @return the head of this list, or {@code null} if this list is empty
 * @since 1.5
 */
public E poll() {
    final Node<E> f = first;
    return (f == null) ? null : unlinkFirst(f);
}

/**
 * Retrieves and removes the head (first element) of this list.
 *
 * @return the head of this list
 * @throws NoSuchElementException if this list is empty
 * @since 1.5
 */
public E remove() {
    return removeFirst();
}

/**
 * Adds the specified element as the tail (last element) of this list.
 *
 * @param e the element to add
 * @return {@code true} (as specified by {@link Queue#offer})
 * @since 1.5
 */
public boolean offer(E e) {
    return add(e);
}
```
- Deque 方法

```java
/**
 * Inserts the specified element at the front of this list.
 *
 * @param e the element to insert
 * @return {@code true} (as specified by {@link Deque#offerFirst})
 * @since 1.6
 */
public boolean offerFirst(E e) {
    addFirst(e);
    return true;
}

/**
 * Inserts the specified element at the end of this list.
 *
 * @param e the element to insert
 * @return {@code true} (as specified by {@link Deque#offerLast})
 * @since 1.6
 */
public boolean offerLast(E e) {
    addLast(e);
    return true;
}

/**
 * Retrieves, but does not remove, the first element of this list,
 * or returns {@code null} if this list is empty.
 *
 * @return the first element of this list, or {@code null}
 *         if this list is empty
 * @since 1.6
 */
public E peekFirst() {
    final Node<E> f = first;
    return (f == null) ? null : f.item;
    }

/**
 * Retrieves, but does not remove, the last element of this list,
 * or returns {@code null} if this list is empty.
 *
 * @return the last element of this list, or {@code null}
 *         if this list is empty
 * @since 1.6
 */
public E peekLast() {
    final Node<E> l = last;
    return (l == null) ? null : l.item;
}

/**
 * Retrieves and removes the first element of this list,
 * or returns {@code null} if this list is empty.
 *
 * @return the first element of this list, or {@code null} if
 *     this list is empty
 * @since 1.6
 */
public E pollFirst() {
    final Node<E> f = first;
    return (f == null) ? null : unlinkFirst(f);
}

/**
 * Retrieves and removes the last element of this list,
 * or returns {@code null} if this list is empty.
 *
 * @return the last element of this list, or {@code null} if
 *     this list is empty
 * @since 1.6
 */
public E pollLast() {
    final Node<E> l = last;
    return (l == null) ? null : unlinkLast(l);
}

/**
 * Pushes an element onto the stack represented by this list.  In other
 * words, inserts the element at the front of this list.
 *
 * <p>This method is equivalent to {@link #addFirst}.
 *
 * @param e the element to push
 * @since 1.6
 */
public void push(E e) {
    addFirst(e);
}

/**
 * Pops an element from the stack represented by this list.  In other
 * words, removes and returns the first element of this list.
 *
 * <p>This method is equivalent to {@link #removeFirst()}.
 *
 * @return the element at the front of this list (which is the top
 *         of the stack represented by this list)
 * @throws NoSuchElementException if this list is empty
 * @since 1.6
 */
public E pop() {
    return removeFirst();
}

/**
 * Removes the first occurrence of the specified element in this
 * list (when traversing the list from head to tail).  If the list
 * does not contain the element, it is unchanged.
 *
 * @param o element to be removed from this list, if present
 * @return {@code true} if the list contained the specified element
 * @since 1.6
 */
public boolean removeFirstOccurrence(Object o) {
    return remove(o);
}

/**
 * Removes the last occurrence of the specified element in this
 * list (when traversing the list from head to tail).  If the list
 * does not contain the element, it is unchanged.
 *
 * @param o element to be removed from this list, if present
 * @return {@code true} if the list contained the specified element
 * @since 1.6
 */
public boolean removeLastOccurrence(Object o) {
    if (o == null) {
        for (Node<E> x = last; x != null; x = x.prev) {
            if (x.item == null) {
                unlink(x);
                return true;
            }
        }
    } else {
        for (Node<E> x = last; x != null; x = x.prev) {
            if (o.equals(x.item)) {
                unlink(x);
                return true;
            }
        }
    }
    return false;
}
```


2. **Set**
    1. **==HashSet==**
![](/Res/images/集合框架-Collection-Set-HashSet层次结构图.png)
- 概述
HashSet 是 Java 中的一个集合类，它实现了 Set 接口，用于存储不重复的元素。它基于 `HashMap` 实现，底层数据结构是`HashTable`，并继承了 HashMap 的一些方法。**需要注意的是，HashSet 中的元素是无序的，并且不允许重复。**
- 底层数据结构
```java
/**
 * The HashMap which backs this Set.
 */
private transient HashMap<T, String> map;
```
- 常用方法
    HashSet 是 Java 中的一个集合类，它实现了 Set 接口，用于存储不重复的元素。它基于 HashMap 实现，并继承了 HashMap 的一些方法。

    **添加元素**
    - `add(E e)`: 将指定的元素添加到 HashSet 中，如果元素已经存在，则不添加并返回 `false`。
    - `addAll(Collection c)`: 将指定集合中的所有元素添加到 HashSet 中。

    **删除元素**

    * `remove(Object o)`: 从 HashSet 中删除指定的元素，如果元素存在则返回 `true`，否则返回 `false`。
    * `clear()`: 清空 HashSet 中的所有元素。

    **判断元素**

    * `contains(Object o)`: 判断 HashSet 中是否包含指定的元素，如果包含则返回 `true`，否则返回 `false`。
    * `isEmpty()`: 判断 HashSet 是否为空，如果为空则返回 `true`，否则返回 `false`。

    **获取信息**

    * `size()`: 返回 HashSet 中元素的数量。
    * `iterator()`: 返回一个迭代器，用于遍历 HashSet 中的元素。

    **其他方法**

    * `toArray()`: 将 HashSet 中的元素转换为数组。
    * `containsAll(Collection c)`: 判断 HashSet 是否包含指定集合中的所有元素。
    * `removeAll(Collection c)`: 从 HashSet 中删除包含在指定集合中的所有元素。
    * `retainAll(Collection c)`: 从 HashSet 中删除所有未包含在指定集合中的元素。
    
    2. **==TreeSet==**
![](/Res/images/集合框架-Collection-Set-TreeSet层次结构图.png)
- 概述
    TreeSet 也是 Java 中的一个集合类，它同样实现了 Set 接口，并继承了 `SortedSet` 接口。TreeSet 基于 `TreeMap` 实现，底层数据结构是**红黑树**,可以对元素进行排序。**需要注意的是，TreeSet 中的元素是有序的，并且不允许重复。元素的排序方式取决于元素的自然顺序或指定的比较器。**
- 底层数据结构
```java
/**
 * The NavigableMap which backs this Set.
 */
// Not final because of readObject. This will always be one of TreeMap or
// TreeMap.SubMap, which both extend AbstractMap.
private transient NavigableMap<T, String> map;
```
- 常用方法
    **添加元素**

    * `add(E e)`: 将指定的元素添加到 TreeSet 中，并根据元素的自然顺序或指定的比较器进行排序。
    * `addAll(Collection c)`: 将指定集合中的所有元素添加到 TreeSet 中。

    **删除元素**

    * `remove(Object o)`: 从 TreeSet 中删除指定的元素。
    * `clear()`: 清空 TreeSet 中的所有元素。

    **判断元素**

    * `contains(Object o)`: 判断 TreeSet 中是否包含指定的元素。
    * `isEmpty()`: 判断 TreeSet 是否为空。
    * `first()`: 返回 TreeSet 中的第一个元素。
    * `last()`: 返回 TreeSet 中的最后一个元素。

    **范围操作**

    * `headSet(E toElement)`: 返回小于指定元素的元素集合。
    * `tailSet(E fromElement)`: 返回大于等于指定元素的元素集合。
    * `subSet(E fromElement, E toElement)`: 返回大于等于 fromElement 且小于 toElement 的元素集合。

    **导航方法**

    * `lower(E e)`: 返回小于指定元素的最大的元素。
    * `higher(E e)`: 返回大于指定元素的最小元素。
    * `floor(E e)`: 返回小于等于指定元素的最大的元素。
    * `ceiling(E e)`: 返回大于等于指定元素的最小的元素。

    **其他方法**

    * `size()`: 返回 TreeSet 中元素的数量。
    * `iterator()`: 返回一个迭代器，用于遍历 TreeSet 中的元素。
    * `descendingIterator()`: 返回一个逆序迭代器，用于逆序遍历 TreeSet 中的元素。

    3. **==LinkedHashSet==**
![](/Res/images/集合框架-Collection-Set-LinkedHashSet层次结构图.png)
- 概述
LinkedHashSet 继承自 HashSet 并实现了 Set 接口。LinkedHashSet 的特点是**维护元素的插入顺序**，也就是说，遍历 LinkedHashSet 中的元素时，会按照元素添加的顺序访问它们。**需要注意的是，LinkedHashSet 中的元素也是不允许重复的**；LinkedHashSet 与 HashSet 的主要区别在于：**LinkedHashSet 维护元素的插入顺序，而 HashSet 不维护。LinkedHashSet 的性能略低于 HashSet，因为需要维护插入顺序。**
- 底层数据结构
LinkedHashSet的底层数据是**哈希表**+**双向链表**
- 常用方法
    **添加元素**

    * `add(E e)`: 将指定的元素添加到 LinkedHashSet 的末尾。
    * `addAll(Collection c)`: 将指定集合中的所有元素添加到 LinkedHashSet 的末尾。

    **删除元素**

    * `remove(Object o)`: 从 LinkedHashSet 中删除指定的元素。
    * `clear()`: 清空 LinkedHashSet 中的所有元素。

    **判断元素**

    * `contains(Object o)`: 判断 LinkedHashSet 中是否包含指定的元素。
    * `isEmpty()`: 判断 LinkedHashSet 是否为空。

    **获取信息**

    * `size()`: 返回 LinkedHashSet 中元素的数量。
    * `iterator()`: 返回一个迭代器，用于按照插入顺序遍历 LinkedHashSet 中的元素。

    **其他方法**

    * `toArray()`: 将 LinkedHashSet 中的元素转换为数组。
    * `containsAll(Collection c)`: 判断 LinkedHashSet 是否包含指定集合中的所有元素。
    * `removeAll(Collection c)`: 从 LinkedHashSet 中删除包含在指定集合中的所有元素。
    * `retainAll(Collection c)`: 从 LinkedHashSet 中删除所有未包含在指定集合中的元素。

3. **Queue**
- Java里有一个叫做Stack的类，却没有叫做Queue的类(它是个接口名字)。当需要使用栈时，Java已不推荐使用`Stack`，而是推荐使用更高效的`ArrayDeque`；既然Queue只是一个接口，当需要使用队列时也就首选`ArrayDeque`了(次选是`LinkedList`)。
- `Deque`是"double ended queue", 表示双向的队列。 Deque 继承自 Queue接口，除了支持Queue的方法之外，还支持insert, remove和examine操作，由于Deque是双向的，所以可以对队列的头和尾都进行操作。Deque既可以当做栈使用，也可以当做队列、双端队列使用。
- `ArrayDeque`和`LinkedList`是`Deque`的两个通用实现，官方更推荐使用`AarryDeque`用作栈和队列，本节总结ArrayDeque
    
    1. **==ArrayDeque==**
- ArrayDeque概述
从名字可以看出ArrayDeque底层通过数组实现，为了满足可以同时在数组两端插入或删除元素的需求，该数组还必须是循环的，即**循环数组(circular array)**，也就是说数组的任何一点都可能被看作起点或者终点。ArrayDeque是**非线程安全**的(not thread-safe)，当多个线程同时使用的时候，需要程序员手动同步；另外，**该容器不允许放入null元素**。
![](/Res/images/集合框架-Collection-Queue-ArrayDeque概述.png)
上图中我们看到，`head`指向首端第一个有效元素，`tail`指向尾端第一个可以插入元素的空位。因为是循环数组，所以`head`不一定总等于0，`tail`也不一定总是比`head`大。
- ArrayQueue常用方法
  - addFirst()
  - addLast()
  - pollFirst()
  - pollLast()
  - peekFirst()
  - peekLast()
- 无语了！有时间再总结

    1. **==PriorityQueue==**

- **概述**
![](/Res/images/集合框架-Collection-Queue-PriorityQueue概述.png)
  - 前面以Java ArrayDeque为例讲解了Stack和Queue，其实还有一种特殊的队列叫做`PriorityQueue`，即优先队列。**优先队列的作用是能保证每次取出的元素都是队列中权值最小的**(Java的优先队列每次取最小元素，C++的优先队列每次取最大元素)。这里牵涉到了大小关系，元素大小的评判可以通过元素本身的自然顺序(natural ordering)，也可以通过构造时传入的比较器(Comparator，类似于C++的仿函数)。
  - Java中PriorityQueue实现了Queue接口，**不允许放入null元素**；其通过堆实现，具体说是通过**完全二叉树(complete binary tree)实现的小顶堆**(任意一个非叶子节点的权值，都不大于其左右子节点的权值)，也就意味着可以通过数组来作为PriorityQueue的底层实现。
  - 父节点和子节点的编号是有联系的，更确切的说父子节点的编号之间有如下关系:
    - `leftNo = parentNo*2+1`
    - `rightNo = parentNo*2+2`
    - `parentNo = (nodeNo-1)/2`
  - 通过上述三个公式，可以轻易计算出某个节点的父节点以及子节点的下标。这也就是为什么可以**直接用数组来存储堆**的原因。
  - PriorityQueue的`peek()`和`element`操作是常数时间，`add()`, `offer()`, 无参数的`remove()`以及`poll()`方法的时间复杂度都是`log(N)`。
- **底层数据结构**
PriorityQueue的底层数据结构是堆，具体是**完全二叉树(complete binary tree)实现的小顶堆**
- **常用方法**
   - add()和offer()
    ![](/Res/images/集合框架-Collection-Queue-PriorityQueue-常用方法add()&offer().png)
    
    add(E e)和offer(E e)的语义相同，都是向优先队列中插入元素，只是**Queue接口规定二者对插入失败时的处理不同**，前者在插入失败时抛出异常，后则则会返回false。对于PriorityQueue这两个方法其实没什么差别。
    
    新加入的元素可能会破坏小顶堆的性质，因此需要进行必要的调整
    ```java
    //offer(E e)
    public boolean offer(E e) {
        if (e == null)//不允许放入null元素
            throw new NullPointerException();
        modCount++;
        int i = size;
        if (i >= queue.length)
            grow(i + 1);//自动扩容
        size = i + 1;
        if (i == 0)//队列原来为空，这是插入的第一个元素
            queue[0] = e;
        else
            siftUp(i, e);//调整
        return true;
    }
    ```
    上述代码中，扩容函数`grow()`类似于`ArrayList`里的`grow()`函数，就是再申请一个更大的数组，并将原数组的元素复制过去，这里不再赘述。需要注意的是`siftUp(int k, E x)`方法，该方法用于插入元素x并维持堆的特性。
    ```java
    //siftUp()
    private void siftUp(int k, E x) {
        while (k > 0) {
            int parent = (k - 1) >>> 1;//parentNo = (nodeNo-1)/2
            Object e = queue[parent];
            if (comparator.compare(x, (E) e) >= 0)//调用比较器的比较方法
                break;
            queue[k] = e;
            k = parent;
        }
        queue[k] = x;
    }
    ```
    新加入的元素x可能会破坏小顶堆的性质，因此需要进行调整。调整的过程为: **从k指定的位置开始，将x逐层与当前点的`parent`进行比较并交换，直到满足`x >= queue[parent]`为止**。注意这里的比较可以是元素的自然顺序，也可以是依靠比较器的顺序。


   - element()和peek()
    ![](/Res/images/集合框架-Collection-Queue-PriorityQueue-常用方法element()&peek().png)
    
    `element()`和`peek()`的语义完全相同，都是**获取但不删除队首元素**，也就是队列中权值最小的那个元素，二者唯一的区别是**当方法失败时前者抛出异常**，**后者返回null**。根据小顶堆的性质，堆顶那个元素就是全局最小的那个；由于堆用数组表示，根据下标关系，0下标处的那个元素既是堆顶元素。所以直接返回数组0下标处的那个元素即可。
    ```java
    //peek()
    public E peek() {
        if (size == 0)
            return null;
        return (E) queue[0];//0下标处的那个元素就是最小的那个
    }
    ```
   - remove()和poll()
    ![](/Res/images/集合框架-Collection-Queue-PriorityQueue-常用方法remove()&poll().png)
    
    remove()和poll()方法的语义也完全相同，都是**获取并删除队首元素**，区别是**当方法失败时前者抛出异常，后者返回null**。由于删除操作会改变队列的结构，为维护小顶堆的性质，需要进行必要的调整。
    
    ```java
    public E poll() {
        if (size == 0)
            return null;
        int s = --size;
        modCount++;
        E result = (E) queue[0];//0下标处的那个元素就是最小的那个
        E x = (E) queue[s];
        queue[s] = null;
        if (s != 0)
            siftDown(0, x);//调整
        return result;
    }
    ```
    上述代码首先记录0下标处的元素，并用最后一个元素替换0下标位置的元素，之后调用`siftDown()`方法对堆进行调整，最后返回原来0下标处的那个元素(也就是最小的那个元素)。重点是`siftDown(int k, E x)`方法，该方法的作用是**从k指定的位置开始，将x逐层向下与当前点的左右孩子中较小的那个交换，直到x小于或等于左右孩子中的任何一个为止**
    ```java
    //siftDown()
    private void siftDown(int k, E x) {
        int half = size >>> 1;
        while (k < half) {
            //首先找到左右孩子中较小的那个，记录到c里，并用child记录其下标
            int child = (k << 1) + 1;//leftNo = parentNo*2+1
            Object c = queue[child];
            int right = child + 1;
            if (right < size &&
                comparator.compare((E) c, (E) queue[right]) > 0)
                c = queue[child = right];
            if (comparator.compare(x, (E) c) <= 0)
                break;
            queue[k] = c;//然后用c取代原来的值
            k = child;
        }
        queue[k] = x;
    }
    ```
   - remove(Object o)
    ![](/Res/images/集合框架-Collection-Queue-PriorityQueue-常用方法remove(Object%20o).png)
    
    `remove(Object o)`方法用于删除队列中跟o相等的某一个元素(如果有多个相等，只删除一个)，该方法不是`Queue`接口内的方法，而是`Collection`接口的方法。由于删除操作会改变队列结构，所以要进行调整；又由于删除元素的位置可能是任意的，所以调整过程比其它函数稍加繁琐。具体来说，`remove(Object o)`可以分为2种情况: 1. **删除的是最后一个元素**。直接删除即可，不需要调整。2. **删除的不是最后一个元素**，从删除点开始以最后一个元素为参照调用一次`siftDown()`即可。此处不再赘述。
    ```java
    //remove(Object o)
    public boolean remove(Object o) {
        //通过遍历数组的方式找到第一个满足o.equals(queue[i])元素的下标
        int i = indexOf(o);
        if (i == -1)
            return false;
        int s = --size;
        if (s == i) //情况1
            queue[i] = null;
        else {
            E moved = (E) queue[s];
            queue[s] = null;
            siftDown(i, moved);//情况2
            ......
        }
        return true;
    }
    ```
#### 1.1.2 Iterator
![](/Res/images/集合框架-Iterator工作机制.png)
#### 1.1.3 Map
1. **HashMap**
- 概述
`HashMap`实现了`Map`接口，即允许放入`key`为`null`的元素，也允许插入`value`为`null`的元素；除该类**未实现同步**外，其余跟`Hashtable`大致相同；跟`TreeMap`不同，该容器不保证元素顺序，根据需要该容器可能会对元素重新哈希，元素的顺序也会被重新打散，因此不同时间迭代同一个HashMap的顺序可能会不同。 根据**对冲突的处理方式**不同，哈希表有两种实现方式，一种**开放地址方式**(Open addressing)，另一种是**冲突链表方式**(Separate chaining with linked lists)。**Java7 HashMap采用的是冲突链表方式。**
![](/Res/images/集合框架-Map-HashMap概述.png)
  - 从上图容易看出，如果选择合适的哈希函数，put()和get()方法可以在常数时间内完成。但在对HashMap进行迭代时，需要遍历整个table以及后面跟的冲突链表。**因此对于迭代比较频繁的场景，不宜将HashMap的初始大小设的过大**。
  - 有两个参数可以影响HashMap的性能: **初始容量(inital capacity)**和**负载系数(load factor)**。**初始容量指定了初始table的大小**，**负载系数用来指定自动扩容的临界值**。当entry的数量超过`capacity*load_factor`时，容器将**自动扩容**并重新哈希。对于**插入元素较多的场景**，将**初始容量设大**可以减少重新哈希的次数。
  - 将对象放入到HashMap或HashSet中时，有两个方法需要特别关心: `hashCode()`和`equals()`。hashCode()方法决定了对象会被放到哪个bucket里，当多个对象的哈希值冲突时，equals()方法决定了这些对象是否是“同一个对象”。所以，如果要将自定义的对象放入到HashMap或HashSet中，需要 **@Override** hashCode()和equals()方法。
- java7方法详解
  - get()
    `get(Object key)`方法根据指定的`key`值返回对应的`value`，该方法调用了`getEntry(Object key)`得到相应的`entry`，然后返回`entry.getValue()`。因此`getEntry()`是算法的核心。 算法思想是首先通过`hash()`函数得到对应bucket的下标，然后依次遍历冲突链表，通过`key.equals(k)`方法来判断是否是要找的那个entry。
    ![](/Res/images/集合框架-Map-HashMap-Java7-get().png)
    上图中`hash(k)&(table.length-1)`等价于`hash(k)%table.length`，原因是HashMap要求`table.length`必须是**2的指数**，因此`table.length-1`就是二进制低位全是1，跟hash(k)相与会将哈希值的高位全抹掉，剩下的就是余数了。
    ```java
    //getEntry()方法
    final Entry<K,V> getEntry(Object key) {
        ......
        int hash = (key == null) ? 0 : hash(key);
        for (Entry<K,V> e = table[hash&(table.length-1)];//得到冲突链表
            e != null; e = e.next) {//依次遍历冲突链表中的每个entry
            Object k;
            //依据equals()方法判断是否相等
            if (e.hash == hash &&
                ((k = e.key) == key || (key != null && key.equals(k))))
                return e;
        }
        return null;
    }
    ```
  - put()
    ![](/Res/images/集合框架-Map-HashMap-Java7-put().png)
  - remove
    ![](/Res/images/集合框架-Map-HashMap-Java7-remove().png)
- java8方法详解
    ![](/Res/images/集合框架-Map-HashMap-Java8概述.png)

1. **TreeMap**
2. **LinkedHashMap**
3. **HashTable**
4. **LinkedHashMap**

### 1.2 IO
![](/Res/images/JavaIO包.png)
![](/Res/images/JavaNIO包.png)
### 1.3 异常处理
![](/Res/images/)
### 1.4 反射
### 1.5 泛型
1. **泛型概述**
   1. **什么是泛型**
   泛型的本质是为了参数化类型（在不创建新的类型的情况下，通过泛型指定的不同类型来控制形参具体限制的类型）。也就是说在泛型使用过程中，操作的数据类型被指定为一个参数，这种参数类型可以用在类、接口和方法中，分别被称为泛型类、泛型接口、泛型方法。
   2. **为什么要引入泛型**
      - 适用于多种数据类型执行相同的代码（代码复用）
      - 泛型中的类型在使用时指定，不需要强制类型转换（类型安全，编译器会检查类型）
2. **泛型的基本使用**
    1. **==泛型类==**
    ```java
    class Point<T>{         // 此处可以随便写标识符号，T是type的简称  
        private T var ;     // var的类型由T指定，即：由外部指定  
        public T getVar(){  // 返回值的类型由外部决定  
            return var ;  
        }  
        public void setVar(T var){  // 设置的类型也由外部决定  
            this.var = var ;  
        }  
    }  
    public class GenericsDemo06{  
        public static void main(String args[]){  
            Point<String> p = new Point<String>() ;     // 里面的var类型为String类型  
            p.setVar("it") ;                            // 设置字符串  
            System.out.println(p.getVar().length()) ;   // 取得字符串的长度  
        }  
    }
    ```
    多元泛型
    ```java
    class Notepad<K,V>{       // 此处指定了两个泛型类型  
        private K key ;     // 此变量的类型由外部决定  
        private V value ;   // 此变量的类型由外部决定  
        public K getKey(){  
            return this.key ;  
        }  
        public V getValue(){  
            return this.value ;  
        }  
        public void setKey(K key){  
            this.key = key ;  
        }  
        public void setValue(V value){  
            this.value = value ;  
        }  
    } 
    public class GenericsDemo09{  
        public static void main(String args[]){  
            Notepad<String,Integer> t = null ;        // 定义两个泛型类型的对象  
            t = new Notepad<String,Integer>() ;       // 里面的key为String，value为Integer  
            t.setKey("汤姆") ;        // 设置第一个内容  
            t.setValue(20) ;            // 设置第二个内容  
            System.out.print("姓名；" + t.getKey()) ;      // 取得信息  
            System.out.print("，年龄；" + t.getValue()) ;       // 取得信息  
    
        }  
    }
    ```
    2. **==泛型接口==**
    ```java
    interface Info<T>{        // 在接口上定义泛型  
        public T getVar() ; // 定义抽象方法，抽象方法的返回值就是泛型类型  
    }  
    class InfoImpl<T> implements Info<T>{   // 定义泛型接口的子类  
        private T var ;             // 定义属性  
        public InfoImpl(T var){     // 通过构造方法设置属性内容  
            this.setVar(var) ;    
        }  
        public void setVar(T var){  
            this.var = var ;  
        }  
        public T getVar(){  
            return this.var ;  
        }  
    } 
    public class GenericsDemo24{  
        public static void main(String arsg[]){  
            Info<String> i = null;        // 声明接口对象  
            i = new InfoImpl<String>("汤姆") ;  // 通过子类实例化对象  
            System.out.println("内容：" + i.getVar()) ;  
        }  
    }
    ```
    3. **==泛型方法==**
    泛型方法，是在调用方法的时候指明泛型的具体类型
    **定义泛型方法：**
    ![](/Res/images/Java基础-高级特性-泛型-泛型方法定义.png)
    **调用泛型方法：**
    ![](/Res/images/Java基础-高级特性-泛型-泛型方法调用.png)
    - 说明一下，定义泛型方法时，必须在返回值前边加一个<T>，来声明这是一个泛型方法，持有一个泛型T，然后才可以用泛型T作为方法的返回值。
    - `Class<T>`的作用就是指明泛型的具体类型，而`Class<T>`类型的变量c，可以用来创建泛型类的对象。
    - 为什么要用变量c来创建对象呢？既然是泛型方法，就代表着我们不知道具体的类型是什么，也不知道构造方法如何，因此没有办法去new一个对象，但可以利用变量c的newInstance方法去创建对象，也就是利用反射创建对象。
    - 泛型方法要求的参数是`Class<T>`类型，而`Class.forName()`方法的返回值也是`Class<T>`，因此可以用`Class.forName()`作为参数。其中，`forName()`方法中的参数是何种类型，返回的`Class<T>`就是何种类型。在本例中，`forName()`方法中传入的是`User`类的完整路径，因此返回的是`Class<User>`类型的对象，因此调用泛型方法时，变量c的类型就是`Class<User>`，因此泛型方法中的泛型T就被指明为`User`，因此变量obj的类型为`User`。
    - 当然，泛型方法不是仅仅可以有一个参数`Class<T>`，可以根据需要添加其他参数。
    - **为什么要使用泛型方法呢**？因为泛型类要在实例化的时候就指明类型，如果想换一种类型，不得不重新new一次，可能不够灵活；而泛型方法可以在调用的时候指明类型，更加灵活。
3. 泛型的上下限
4. 类型擦除
   1. **==什么是类型擦除==**
    
- 泛型的本质是将**数据类型参数化**，它通过**擦除**的方式来**实现**，即编译器会在编译期间擦除代码中的所有泛型语法并相应的做出一些**类型转换**动作。
- 换而言之，泛型信息只存在于**代码编译阶段**，在代码编译结束后，与泛型相关的信息会被擦除掉，专业术语叫做类型擦除。也就是说，成功编译过后的 class 文件中不包含任何泛型信息，泛型信息不会进入到运行时阶段。
   
   1. **==类型擦除的原理==**
1. 泛型通配符
### 1.6 序列化
### 1.7 注解
### 1.8 lambda表达式
### 1.9 字符串处理
### 1.10 网络编程

## 二、八股问题整理(高级特性与基础部分其实都是基础部分，有混杂的问题，后序待修订使条理更清晰)

### 集合
#### (1).ArrayList、Vector、LinkedList，默认扩容大小
#### (11)ArrayList、Vector和linkedList的区别
#### (12)HashMap和HashTable的区别
#### (13)Collection包结构，与Collections的区别
#### (31)说说List,Set,Map三者的区别？
#### (35)说说什么是 fail-fast？
#### (53)fail-fast和fail-safe迭代器的区别是什么？
#### (36)HashMap 中的 key 我们可以使用任何类作为 key 吗？
#### (37)HashMap 的长度为什么是 2 的 N 次方呢？
#### (38)HashMap 与 ConcurrentHashMap 的异同
#### (39)红黑树的特征
#### (54)HashMap底层

### IO流
#### (28)说说Java 中 IO 流
#### (29)Java IO与 NIO的区别（补充）

### 异常处理
#### (23)try catch finally，try里有return，finally还执行么？
#### (40)说说你平时是怎么处理 Java 异常的
#### (50)简述throw与throws的区别
#### (47)出现在Java程序中的finally代码块是否一定会执行？


### 反射
#### (30)java反射的作用与原理
#### (33)获取一个类Class对象的方式有哪些？(java反射部分30)

### 泛型
#### (15)泛型常用特点
#### 为什么需要泛型？
#### 泛型类如何定义使用？
#### 泛型接口如何定义使用？
#### 泛型方法如何定义使用？
#### 泛型的上限和下限？
#### 如何理解Java中的泛型是伪泛型？
### 序列化
#### (49)序列化是什么？
#### (27)Java 序列化中如果有些字段不想进行序列化，怎么办？
#### (51)简述Java序列化与反序列化的实现
#### (41).Java 中，Serializable 与 Externalizable 的区别?
Serializable 接口是一个序列化 Java 类的接口，以便于它们可以在网络上传输或者可以将它们的状态保存在磁盘上，是 JVM 内嵌的默认序列化方式，成本高、脆弱而且不安全。Externalizable 允许你控制整个序列化过程，指定特定的二进制格式，增加安全机制

### 注解
#### 注解的作用？
#### 注解的常见分类？
### lambda表达式

### 字符串处理
#### (10)String、StringBuffer、StringBuilder的区别

### 网络编程

#### (25)OOM你遇到过哪些情况，SOF你遇到过哪些情况

#### (52)Java中线程安全的基本数据结构有哪些

#### (55)concurrentHashMap是如何实现线程安全的(put 如下；get volatile)

> from pdai


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
