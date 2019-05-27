# java 集合框架

## List

### 1. ArrayList

ArrayList基于动态数组实现，支持随机访问；



但是删除元素时需要调用 `System.arraycopy()` 将 `index+1` 后面的元素都复制到 index 位置上，该操作的时间复杂度为 O(N)，可以看出 ArrayList 删除元素的代价是非常高的；



添加元素时使用 ensureCapacityInternal() 方法来保证容量足够，如果不够时，需要使用 grow() 方法进行扩容，新容量的大小为 `oldCapacity + (oldCapacity >> 1)`，也就是旧容量的 1.5 倍。扩容操作需要调用 `Arrays.copyOf()` 把原数组整个复制到新数组中，这个操作代价很高，因此最好在创建 ArrayList 对象时就指定大概的容量大小。



### 2.Vector

#### 同步

它的实现与 ArrayList 类似，但是使用了 synchronized 进行同步。

#### 与 ArrayList 的比较

- Vector 是同步的，因此开销就比 ArrayList 要大，访问速度更慢。最好使用 ArrayList 而不是 Vector，因为同步操作完全可以由程序员自己来控制；
- Vector 每次扩容请求其大小的 2 倍空间，而 ArrayList 是 1.5 倍。

#### 解决线程安全的替代方案

ArrayList并不是线程安全的，所以可以使用`Collections.synchronizedList();` 得到一个线程安全的 ArrayList。

```java
List<String> list = new ArrayList<>();
List<String> synList = Collections.synchronizedList(list);

```

也可以使用 concurrent 并发包下的 `CopyOnWriteArrayList` 类。



CopyOnWriteArrayList采用读写分离的方式：

- 写操作在一个复制的数组上进行，读操作还是在原始数组中进行，读写分离，互不影响。

- 写操作需要加锁，防止并发写入时导致写入数据丢失;写操作结束之后需要把原始数组指向新的复制数组。

  

```java
List<String> list = new CopyOnWriteArrayList<>();Copy to clipboardErrorCopied
```

#### CopyOnWriteArrayList适用场景

CopyOnWriteArrayList 在写操作的同时允许读操作，大大提高了读操作的性能，因此很适合读多写少的应用场景。

但是 CopyOnWriteArrayList 有其缺陷：

- 内存占用：在写操作时需要复制一个新的数组，使得内存占用为原来的两倍左右；
- 数据不一致：读操作不能读取实时性的数据，因为部分写操作的数据还未同步到读数组中。

所以 CopyOnWriteArrayList 不适合内存敏感以及对实时性要求很高的场景。



### 3.LinkedList

#### 实现

LinkedList 基于双向链表实现，静态内部类Node保存节点信息，

```java
private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```

并且链表储存了first 和 last 指针

```java
    transient Node<E> first;

    transient Node<E> last;
```

#### 与 ArrayList 的比较

- ArrayList 基于动态数组实现，LinkedList 基于双向链表实现；
- ArrayList 支持随机访问，LinkedList 不支持；
- LinkedList 在任意位置添加删除元素更快。



## Map

### 1.HashMap

