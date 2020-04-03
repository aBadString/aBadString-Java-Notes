<!-- TOC -->

- [1 String、StringBuffer、StringBuilder](#1-stringstringbufferstringbuilder)
- [2 HashMap](#2-hashmap)
- [3 ConcurrentHashMap](#3-concurrenthashmap)
    - [3.1 jdk 1.7 源码](#31-jdk-17-源码)
    - [3.2 jdk 1.8 源码](#32-jdk-18-源码)
- [4 ReentrantLock](#4-reentrantlock)
- [5 AbustactQueuedSynchronizer](#5-abustactqueuedsynchronizer)
    - [5.1 同步状态](#51-同步状态)
    - [5.2 同步队列](#52-同步队列)
    - [5.3 源码分析](#53-源码分析)

<!-- /TOC -->

# 1 String、StringBuffer、StringBuilder

- String 字符串常量（不可变）
- StringBuffer 字符串变量（线程安全）
- StringBuilder 字符串变量（非线程安全）

String 是不可变的对象。在每次对 String 类型进行改变的时候其实都等同于生成了一个新的 String 对象，然后将指针指向新的 String 对象，非常耗费性能。

StringBuffer 则是每次都会对 StringBuffer 对象本身进行操作。

另外像这种的 String S1 = "hello" + "java"; 是直接编译时拼接的。

```java
@Test
public void test() {
	String s = "hellojava";
	String s1 = "hello";
	String s2 = "java";
	// 编译时就会将字符串拼接，和s没有区别
	String s3 = "hello" + "java";
	// 只有有变量参与字符串拼接，
	// 那么就会调用StringBuilder中的toString方法
	// 创建一个新的Sting对象。
	String s4 = s1 + "java";
	String s5 = "hello" + s2;
	String s6 = s1 + s2;
	// 直接去内存中的常量池中获取该字符串对象
	String s7 = s6.intern();
	System.out.println(s == s3); // true
	System.out.println(s == s4); // false
	System.out.println(s == s5); // false
	System.out.println(s == s6); // false
	System.out.println(s4 == s5); // false
	System.out.println(s4 == s6); // false
	System.out.println(s == s7); // true
}
```

在大部分情况下 StringBuilder > StringBuffer > String

# 2 HashMap

HashMap 是一个散列表，存储着键值对映射。

```java
public class HashMap<K,V> 
    extends AbstractMap<K,V> 
    implements Map<K,V>, Cloneable, Serializable 
```

Map是 key-value 键值对 接口

AbstractMap 实现了 键值对 的通用函数接口



它是线程不安全的，key 和 value 都可以为 null，并且它是无序的。

两个参数：初始容量和加载因子。

容量是HashMap中桶的数量；加载因子是HashMap在其容量自动增加之前可以达到多满的一种尺度。当键值对的数量超过加载因子与当前容量之积（即使用了百分之多少的桶），就要对HashMap进行 rehash 操作（重建内部数据结构），重建之后HashMap讲具有大约之前两倍的桶数目。

**构造器 (4个)**

```java
// 默认构造函数.
/**
 * 使用默认初始容量(16)和默认负载因子(0.75)构造一个空的Hashmap.
 */
public HashMap() {
    // 所有其他字段默认
    this.loadFactor = DEFAULT_LOAD_FACTOR;
}

// 指定“容量大小”的构造函数
/**
 * 使用指定的初始容量和默认的负载因子(0.75)构造一个空的HashMap.
 * @param initialCapacity 初始容量.
 * @throws IllegalArgumentException 如果初始容量是负的.
 */
public HashMap(int initialCapacity) {
    this(initialCapacity, DEFAULT_LOAD_FACTOR);
}

// 指定“容量大小”和“加载因子”的构造函数
/**
 * 使用指定的初始容量和负载因子构造一个空的HashMap.
 * @param  initialCapacity 初始容量.
 * @param  loadFactor 负载因子.
 * @throws IllegalArgumentException 如果初始容量为负，或负载因子为非正.
 */
public HashMap(int initialCapacity, float loadFactor) {
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " +
                                           initialCapacity);
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " +
                                           loadFactor);
    this.loadFactor = loadFactor;
    this.threshold = tableSizeFor(initialCapacity);
}

// 包含“子Map”的构造函数
/**
 * 使用与指定映射相同的映射构造新的HashMap。HashMap是使用默认的负载因子(0.75)和足够容纳指定映射的初始容量创建的.
 * @param   m the map whose mappings are to be placed in this map
 * @throws  NullPointerException 如果指定的映射为空.
 */
public HashMap(Map<? extends K, ? extends V> m) {
    this.loadFactor = DEFAULT_LOAD_FACTOR;
    putMapEntries(m, false);
}
```



**1、 HashMap的内部数据结构**

jdk 1.8 ：内部使用数组 + 链表红黑树

![img](https://pic3.zhimg.com/80/v2-4ce38fd1c36fc61e70d7687fae997e5a_720w.jpg) 

当链表长度大于 8 时，转为红黑树；当红黑树节点小于 6 时，转为链表。

**2、HashMap的数据插入原理**

1. 判断数组是否为空。
   1. 为空，则初始化数组
   2. 不为空，继续。
2. 计算 key 的 hash 值，通过`(n - 1) & hash`计算应当存放在数组中的下标 index
3. 判断该位置是否存在数据
   1. 不存在，则放置要插入的数据。
   2. 存在，继续
4. 判断 key 值是否相等
   1. 相等，更新 value。
   2. 不相等，继续
5. 查看当前位置是红黑树还是链表
   1. 是红黑树，则放入红黑树节点
   2. 是链表，则加入链表中
6. 链表则要判断是否要转为红黑树。
7. 红黑树则要判断是否需要扩容。

![img](https://pic4.zhimg.com/80/v2-ff5fe3aac820fafb4cf34b2a801877cf_720w.jpg)



**3、HashMap怎么设定初始容量大小**

默认初始容量大小是16，负载因子是 0.75。

如果传入初始容量 n，则初始容量设定为 大于等于 k 的 2的整数次幂。

**4、哈希函数的设计**

先取 key 的 hashcode()，让其高16位和低16位进行异或操作。

扰动函数：

1. 尽可能降低hash碰撞，越分散越好；
2. 算法一定要尽可能高效，因为这是高频操作, 因此采用位运算；

```java
bucketIndex = indexFor(hash, table.length);

static int indexFor(int h, int length) {
     return h & (length-1);
}
```

h % length  ==  h & (length-1)

为啥 length 要是 2的次幂。



**5、jdk 1.8 后的改进**

1. 数组+链表 改进为 数组+链表或红黑树
2. 链表插入方式 头插法 改进为 尾插法
3. 插入数据时，1.7 先判断是否要扩容，再插入；1.8 先插入，再判断
4. 1.7 扩容时需要对原数组的元素重新进行 哈希定位；1.8 则要么位置不变，要么向后移动一个旧容量大小。

好处？

1. 使用红黑树：防止发生hash冲突，链表长度过长，将时间复杂度由`O(n)`降为`O(logn)`
2. 头插法会使链表发生反转，多线程环境下会产生环



**6、那HashMap是线程安全的吗？**

不是，在多线程环境下，1.7 会产生死循环、数据丢失、数据覆盖的问题，1.8 中会有数据覆盖的问题。

线程安全的：HashTable、ConcurrentHashMap、Collections.synchronizedMap

- HashTable是直接在操作方法上加synchronized关键字，锁住整个数组，锁粒度比较大。
- Collections.synchronizedMap是使用Collections集合工具的内部类，通过传入Map封装出一个SynchronizedMap对象，内部定义了一个对象锁，方法通过对象锁实现线程安全。
- ConcurrentHashMap使用分段锁，降低了锁粒度，让并发度大大提高。



**7、HashMap内部节点是有序的吗？**

是无序的，根据hash值随机插入

LinkedHashMap 是有序的，内部使用一个链表，有头尾节点。

同时LinkedHashMap节点Entry内部除了继承HashMap的Node属性，还有before 和 after用于标识前置节点和后置节点。可以实现按插入的顺序或访问顺序排序。



# 3 ConcurrentHashMap

线程安全，并且锁分离。

ConcurrentHashMap内部使用段(Segment)来表示这些不同的部分，每个段其实就是一个小的HashTable，它们有自己的锁。

只要多个修改操作发生在不同的段上，它们就可以并发进行。

ConcurrentHashMap使用分段锁，降低了锁粒度，让并发度大大提高。



## 3.1 jdk 1.7 源码

```java
//默认的数组大小16(HashMap里的那个数组)
static final int DEFAULT_INITIAL_CAPACITY = 16;

//扩容因子0.75
static final float DEFAULT_LOAD_FACTOR = 0.75f;

//ConcurrentHashMap中的数组
final Segment<K,V>[] segments;

//默认并发标准16
static final int DEFAULT_CONCURRENCY_LEVEL = 16;

//Segment是ReentrantLock子类，因此拥有锁的操作
static final class Segment<K,V> extends ReentrantLock implements Serializable {
    //HashMap的那一套，分别是数组、键值对数量、阈值、负载因子
    transient volatile HashEntry<K,V>[] table;
    transient int count;
    transient int threshold;
    final float loadFactor;

    Segment(float lf, int threshold, HashEntry<K,V>[] tab) {
        this.loadFactor = lf;
        this.threshold = threshold;
        this.table = tab;
    }
}

//换了马甲还是认识你！！！HashEntry对象，存key、value、hash值以及下一个节点
static final class HashEntry<K,V> {
    final int hash;
    final K key;
    volatile V value;
    volatile HashEntry<K,V> next;
}
//segment中HashEntry[]数组最小长度
static final int MIN_SEGMENT_TABLE_CAPACITY = 2;

//用于定位在segments数组中的位置，下面介绍
final int segmentMask;
final int segmentShift;
```

**构造器：**

```java
public ConcurrentHashMap() {
    this(
        DEFAULT_INITIAL_CAPACITY, 
        DEFAULT_LOAD_FACTOR, 	
        DEFAULT_CONCURRENCY_LEVEL
    );
}

public ConcurrentHashMap(
    int initialCapacity,
    float loadFactor, 
    int concurrencyLevel
) {
    if (!(loadFactor > 0) || initialCapacity < 0 || concurrencyLevel <= 0)
        throw new IllegalArgumentException();
    if (concurrencyLevel > MAX_SEGMENTS)
        concurrencyLevel = MAX_SEGMENTS;
    // Find power-of-two sizes best matching arguments
    //步骤① start
    int sshift = 0;
    int ssize = 1;
    while (ssize < concurrencyLevel) {
        ++sshift;
        ssize <<= 1;
    }
    this.segmentShift = 32 - sshift;
    this.segmentMask = ssize - 1;
    //步骤① end
    //步骤② start
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    int c = initialCapacity / ssize;
    if (c * ssize < initialCapacity)
        ++c;
    int cap = MIN_SEGMENT_TABLE_CAPACITY;
    while (cap < c)
        cap <<= 1;
    //步骤② end
    // create segments and segments[0]
    //步骤③ start
    Segment<K,V> s0 =
        new Segment<K,V>(loadFactor, (int)(cap * loadFactor),
                         (HashEntry<K,V>[])new HashEntry[cap]);
    Segment<K,V>[] ss = (Segment<K,V>[])new Segment[ssize];
    UNSAFE.putOrderedObject(ss, SBASE, s0); // ordered write of segments[0]
    this.segments = ss;
    //步骤③ end
}

// 作者：HuYounger
// 链接：https://juejin.im/post/5a2f2f7851882554b837823a
```

 **put 方法：**

```java
public V put(K key, V value) {
    Segment<K,V> s;
    //步骤①注意valus不能为空！！！
    if (value == null)
        throw new NullPointerException();
    //根据key计算hash值，key也不能为null，否则hash(key)报空指针
    int hash = hash(key);
    //步骤②派上用场了，根据hash值计算在segments数组中的位置
    int j = (hash >>> segmentShift) & segmentMask;
    //步骤③查看当前数组中指定位置Segment是否为空
    //若为空，先创建初始化Segment再put值，不为空，直接put值。
    if ((s = (Segment<K,V>)UNSAFE.getObject // nonvolatile; recheck
         (segments, (j << SSHIFT) + SBASE)) == null) //  in ensureSegment
        s = ensureSegment(j);
    return s.put(key, hash, value, false);
}

// 作者：HuYounger
// 链接：https://juejin.im/post/5a2f2f7851882554b837823a
```

**ensureSegment() 方法：**

```java
private Segment<K,V> ensureSegment(int k) {
    //获取segments
    final Segment<K,V>[] ss = this.segments;
    long u = (k << SSHIFT) + SBASE; // raw offset
    Segment<K,V> seg;
    if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u)) == null) {
        //拷贝一份和segment 0一样的segment
        Segment<K,V> proto = ss[0]; // use segment 0 as prototype
        //大小和segment 0一致，为2
        int cap = proto.table.length;
        //负载因子和segment 0一致，为0.75
        float lf = proto.loadFactor;
        //阈值和segment 0一致，为1
        int threshold = (int)(cap * lf);
        //根据大小创建HashEntry数组tab
        HashEntry<K,V>[] tab = (HashEntry<K,V>[])new HashEntry[cap];
        //再次检查
        if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u))
            == null) { // recheck
            根据已有属性创建指定位置的Segment
                Segment<K,V> s = new Segment<K,V>(lf, threshold, tab);
            while ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u))
                   == null) {
                if (UNSAFE.compareAndSwapObject(ss, u, null, seg = s))
                    break;
            }
        }
    }
    return seg;
}
```





**jdk 7 ConcurrentHashMap put 流程**

![img](https://user-gold-cdn.xitu.io/2017/12/12/1604851702c89293?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 3.2 jdk 1.8 源码

```java
/**
 * Creates a new, empty map with the default initial table size (16).
 * 使用默认的初始表大小（16）创建一个新的空Map。
 */
public ConcurrentHashMap() {
}
```

构造时不会初始化数组

put 操作

```java
public V put(K key, V value) {
    return putVal(key, value, false);
}

final V putVal(K key, V value, boolean onlyIfAbsent) {
    // 不允许 key 和 value 为空
    if (key == null || value == null) 
        throw new NullPointerException();
    
    // 哈希值计算
    int hash = spread(key.hashCode());
    /*
    static final int HASH_BITS = 0x7fffffff; // 31位
    static final int spread(int h) {
    	// 扰动函数
        return (h ^ (h >>> 16)) & HASH_BITS;
    }
    */
    
    int binCount = 0;
    for (Node<K,V>[] tab = table;;) {   
        Node<K,V> f; int n, i, fh;
        
        // 1、初始化 tab
        if (tab == null || (n = tab.length) == 0)
            tab = initTable();
        
        // 2、直接插入节点
        // 判断待插入位置是否为 null，
        // 为空则通过创建新节点，通过CAS方法设置在指定位置
        else if 
        	( 
            	( 
                	f = tabAt(tab, i = (n - 1) & hash) 
            	) == null 
        	) 
        {
            // 插入节点
            if 
            (
                casTabAt
                (
                    tab, i, null,
                    new Node<K,V>(hash, key, value, null)
                )
            )
            {
                // 插入成功则退出循环
                // 到 addCount(1L, binCount); 那里
                break; // no lock when adding to empty bin
            }
        }
        
        // 3、当前节点正在扩容
        else if ((fh = f.hash) == MOVED)
            tab = helpTransfer(tab, f);
        
        // 4、插入位置不为空
        else {
            V oldVal = null;
            synchronized (f) {
                if (tabAt(tab, i) == f) {
                    // 4.1、链表
                    if (fh >= 0) {
                        binCount = 1;
                        // 遍历链表
                        for (Node<K,V> e = f;; ++binCount) {
                            K ek;
                            // 更新值
                            if 
                              (han
                                e.hash == hash // 哈希冲突了
                                &&
                                ( 
                                    // key 相同
                                    // 用 == 和 equals 两种判断
                                    (ek = e.key) == key 
                                    ||
                                    ( ek != null && key.equals(ek) )
                                )
                              ) 
                            {
                                oldVal = e.val;
                                if (!onlyIfAbsent) // onlyIfAbsent 为 false
                                    // 替换旧值
                                    e.val = value;
                                break;
                            }
                            
                            Node<K,V> pred = e;
                            // 到达链表尾部
                            if ((e = e.next) == null) {
                                // 插入值
                                pred.next = new Node<K,V>(hash, key, value, null);
                                break;
                            }
                        }
                        // 遍历链表结束
                    }
                    // 4.2、红黑树
                    else if (f instanceof TreeBin) {
                        Node<K,V> p;
                        binCount = 2;
                        
                        if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key, value)) != null) 					      {
                            oldVal = p.val;
                            if (!onlyIfAbsent)
                                p.val = value;
                        }
                    }
                    
                }
            } // synchronized 结束
            
            if (binCount != 0) {
                // 链表中节点个数超过8转成红黑树
                if (binCount >= TREEIFY_THRESHOLD)
                    treeifyBin(tab, i);
                
                if (oldVal != null)
                    return oldVal;
                break;
            }
        } // 4、插入位置不为空 结束
    } // for结束
    
    
    addCount(1L, binCount);
    return null;
}
```

```java
private final Node<K,V>[] initTable() {
    Node<K,V>[] tab; int sc;
    while ((tab = table) == null || tab.length == 0) {
        // sizeCtl小于0，当前线程让出初始化执行权
        if ((sc = sizeCtl) < 0)
            Thread.yield(); 
        	// lost initialization race; just spin
        	// 失去了初始化竞赛;只是自旋
        
        // CAS 操作将 sizeCtl = -1
        else if (U.compareAndSwapInt(this, SIZECTL, sc, -1)) {
            try {
                // 再次判断table是否为空, 和单例模式一个道理
                if ((tab = table) == null || tab.length == 0) {
                    // 数组大小 sc 
                    // 小于0 使用默认大小 16
                    int n = (sc > 0) ? sc : DEFAULT_CAPACITY;
                    @SuppressWarnings("unchecked")
                    // 这里创建数组
                    Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n];
                    table = tab = nt;
                    sc = n - (n >>> 2);
                }
            } finally {
                sizeCtl = sc;
            }
            break;
        }
    }
    return tab;
}
```



# 4 ReentrantLock

`ReentrantLock` 意思为**可重入锁**，指的是一个线程能够对一个临界资源重复加锁。

![image-20200402181328804](images/image-20200402181328804.png) 

```java
// 默认使用 非公平锁
public ReentrantLock() {
    sync = new NonfairSync();
}

// 带参 true 使用 公平锁
public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}
```

公平锁 FairSync

```java
static final class FairSync extends Sync {
    private static final long serialVersionUID = -3000897897090466540L;

    final void lock() {
        acquire(1);
    }

    /**
     * Fair version of tryAcquire.  Don't grant access unless
     * recursive call or no waiters or is first.
     */
    protected final boolean tryAcquire(int acquires) {
        final Thread current = Thread.currentThread();
        // 获取锁状态
        int c = getState();
        // 如果当前没有其他线程获得锁
        if (c == 0) {
            if (
                !hasQueuedPredecessors() // 如果队列种有线程等待,返回false
                && 
                compareAndSetState(0, acquires) // 没有,则获取锁
               ) 
            {
                // 获取成功则将当前线程置为获得锁的独占线程
                setExclusiveOwnerThread(current);
                return true;
            }
        }
        // 如果锁已经被占用, 但是获取锁的是当前线程 —— 重进入
        else if (current == getExclusiveOwnerThread()) {
            // state + 1，并将值更新
            int nextc = c + acquires;
            if (nextc < 0)
                throw new Error("Maximum lock count exceeded");
            setState(nextc);
            return true;
        }
        return false;
    }
}
```

非公平锁 NonfairSync

```java
static final class NonfairSync extends Sync {
    private static final long serialVersionUID = 7316153563782823691L;

    /**
     * Performs lock.  Try immediate barge, backing up to normal
     * acquire on failure.
     */
    final void lock() {
        // 直接尝试获取锁 --- 甚至都不用 先acquire，太不公平啦！！！
        if (compareAndSetState(0, 1))
            // 获取成功则将当前线程置为获得锁的独占线程
            setExclusiveOwnerThread(Thread.currentThread());
        else
            acquire(1);
    }

    protected final boolean tryAcquire(int acquires) {
        return nonfairTryAcquire(acquires);
    }
}

final boolean nonfairTryAcquire(int acquires) {
    final Thread current = Thread.currentThread();
    int c = getState();
    // 如果当前没有其他线程获得锁
    if (c == 0) {
        // 直接取获取锁 --- 不公平！！！
        if (compareAndSetState(0, acquires)) {
            setExclusiveOwnerThread(current);
            return true;
        }
    }
    // 如果锁已经被占用, 但是获取锁的是当前线程 —— 重进入
    else if (current == getExclusiveOwnerThread()) {
        int nextc = c + acquires;
        if (nextc < 0) // overflow
            throw new Error("Maximum lock count exceeded");
        setState(nextc);
        return true;
    }
    return false;
}
```



释放锁

```java
public void unlock() {
    sync.release(1);
}


protected final boolean tryRelease(int releases) {
    int c = getState() - releases;
    // 当前线程不是占锁的线程
    // 出错啦！
    if (Thread.currentThread() != getExclusiveOwnerThread())
        throw new IllegalMonitorStateException();
    boolean free = false;
    // 当 status减为 0 时
    if (c == 0) {
        // 返回 true
        free = true;
        // 取消独占
        setExclusiveOwnerThread(null);
    }
    // 更新 status
    setState(c);
    return free;
}
```



# 5 AbustactQueuedSynchronizer

AbustactQueuedSynchronizer 是J ava 提供的底层同步工具类，用一个int类型的变量表示同步状态，并提供了一系列的 CAS 操作来管理这个同步状态。

## 5.1 同步状态

AQS 维护了一个 int 类型的变量来表示同步状态。

```java
// 同步状态
private volatile int state;
```

| 方法名                                                       | 描述                         |
| ------------------------------------------------------------ | ---------------------------- |
| `protected final int getState()`                             | 获取`state`的值              |
| `protected final void setState(int newState)`                | 设置`state`的值              |
| `protected final boolean compareAndSetState(int expect, int update)` | 使用`CAS`方式更新`state`的值 |

独占模式：一个线程在进行某些操作时，其他线程不能执行该操作。

共享模式：可以允许多个线程同时进行某种操作。

通过修改 state 字段代表的同步状态来实现多线程的独占模式或者共享模式。



需要自定义获取同步状态与释放同步状态的方式

| 方法名                                        | 描述                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| `protected boolean tryAcquire(int arg)`       | 独占式的获取同步状态，获取成功返回true，否则false            |
| `protected boolean tryRelease(int arg)`       | 独占式的释放同步状态，释放成功返回true，否则false            |
| `protected int tryAcquireShared(int arg)`     | 共享式的获取同步状态，获取成功返回true，否则false            |
| `protected boolean tryReleaseShared(int arg)` | 共享式的释放同步状态，释放成功返回true，否则false            |
| `protected boolean isHeldExclusively()`       | 在独占模式下，如果当前线程已经获取到同步状态，则返回 true；其他情况则返回 false |

如果我们自定义的同步工具需要在独占模式下工作，那么我们就重写`tryAcquire`、`tryRelease`和`isHeldExclusively`方法;

如果是在共享模式下工作，那么我们就重写`tryAcquireShared`和`tryReleaseShared`方法。

## 5.2 同步队列

AQS 还维护了一个同步队列。

队列的节点定义如下，是一个静态内部类

```java
static final class Node {
    volatile int waitStatus;
    volatile Node prev;     // 指向先前节点
    volatile Node next;     // 指向后继节点
    volatile Thread thread; // 每个节点代表一个线程
    Node nextWaiter;
    
    static final int CANCELLED =  1; // 节点对应的线程已经被取消了
    static final int SIGNAL    = -1; // 表示后边的节点对应的线程处于等待状态
    static final int CONDITION = -2; // 表示节点在等待队列中
    static final int PROPAGATE = -3; // 表示下一次共享式同步状态获取将被无条件的传播下去
}
```

定义了两个属性，头节点和尾节点

```java
private transient volatile Node head;
private transient volatile Node tail;
```

这个队列其实是一个带头节点的链表，会有一个空的0号节点

> 强哥的
> AQS通过一个由volatile修饰的int类型的state变量来控制线程获取和释放同步状态的过程。
> 首先AQS里有两种模式
> 独占模式和共享模式
> 然后我讲一下AQS独占模式的获取和释放同步状态的过程。
> 首先我们需要定义获取和释放同步状态的方法，定义好后通过AQS提供的模板方法acquire来获取，这个方法首先调用我们自己定义的tryAcquire方法获取同步状态，如果返回true那么就获取成功，如果不成功。那么会调用addWaiter方法，这个方法主要是通过一个同步队列来调度线程，核心是一个Node双向链表。首先将线程信息封装到一个node结点里面然后插入这个队列的尾部，这个时候如果头尾节点没有初始化会先调用enq方法初始化。如果插入的前一个结点是头节点会再次tryAcquire尝试获取同步状态，主要是希望前面的线程马上就释放同步状态。如果失败就会调用`shouldParkAfterFailedAcquire`方法来修改节点中的`waitStatus`变量信息。（通过循环操作）这里会先将头节点`waitStatus`赋值为-1，然后在将当前节点阻塞。
> 释放过程的话是release方法调用我们编写的tryRelease方法，先释放同步状态，然后修改同步队列中节点的信息，主要是将头节点的waitStatus设置为0，然后唤醒后面的节点。并且将当前节点设置为头节点，thread设置为null。
> 这是独占模式下的情况，如果是共享模式的话，主要的区别是state变量的值。在独占模式下只能为0和-1如果是共享模式，由初始值递减当其值为-1的时候在将后面的线程阻塞。

为什么释放的时候从队尾往前找？

因为队列前面的线程等待时间都比较长，很大可能已经取消等待了。从前往后的话需要遍历次数较多。



## 5.3 源码分析

获取锁 acquire

```java
public final void acquire(int arg) {
    if (
        !tryAcquire(arg) 
        // 如果获取锁成功, 则方法返回, 不执行acquireQueued
        // 如果失败, 执行acquireQueued 后 线程中断
        &&
        acquireQueued(addWaiter(Node.EXCLUSIVE), arg)
    )
    {
        // 中断当前线程
        selfInterrupt();
    }
}
```

```java
// 把节点添加到队列尾部
private Node addWaiter(Node mode) {
    // 将当前线程装入 队列的节点中
    Node node = new Node(Thread.currentThread(), mode);
    // Try the fast path of enq; backup to full enq on failure
    Node pred = tail;
    // 如果尾节点非空, 说明队列已经初始化过啦
    if (pred != null) {
        // 待插入节点 的前驱 是 队列尾节点
        node.prev = pred;
        // 尾指针指向 当前待插入的节点
        if (compareAndSetTail(pred, node)) {
            // 队列尾节点 后继 是 当前节点
            pred.next = node;
            return node;
        }
    }
    // 如果尾节点为空, 说明队列还没有初始化
    // 那么初始化队列
    enq(node);
    return node;
}

// 初始化队列
private Node enq(final Node node) {
    // 循环 保证一定能写入队列
    for (;;) {
        Node t = tail;
        // 第一遍循环时, 尾指针为空
        if (t == null) { // Must initialize
            // 创建 0 号节点
            // 头指针指向 0 号节点
            if (compareAndSetHead(new Node()))
                // 尾指针指向 0 号节点
                tail = head;
        } 
        // 第二遍循环, 尾指针非空, 指向 0 号节点
        else {
            // 当前待插入的节点 前驱 是 0 号节点
            node.prev = t;
            // 尾指针指向 当前待插入的节点
            if (compareAndSetTail(t, node)) {
                // 当前新插入的节点 的后继为 自己 ？？？
                t.next = node;
                // 返回刚刚插入的节点
                return t;
            }
        }
    }
}
```

```java
final boolean acquireQueued(final Node node, int arg) {
    boolean failed = true;
    try {
        boolean interrupted = false;
        
        // 这是一个忙循环
        // 只有当前线程出队列了才能退出
        // 这个循环就是自旋的忙循环
        for (;;) 
        {
            // p 为 node 节点的前驱
            final Node p = node.predecessor();
            
            if (
                p == head // 前驱是 0 号节点, 即自己是1号节点
                && 
                tryAcquire(arg) // 最后再看一眼能不能获取锁
            ) 
            {
                // 如果 当前节点 在队首，且可以获取锁了
                
                // 把当前节点设为 0 号节点
                // 即 出队列
                setHead(node);
                p.next = null; // help GC
                failed = false;
                
                // 不需要中断
                return interrupted;
            }
            
            // 不在队首 或者 不能获取锁
            if (
                // 如果当前节点需要挂起
                // shouldParkAfterFailedAcquire 返回当前线程是否需要挂起，
                // 如果需要则调用 parkAndCheckInterrupt()：
                shouldParkAfterFailedAcquire(p, node) 
                &&
                parkAndCheckInterrupt())
            {
                // 需要中断
                interrupted = true;
            }
        }
    } finally {
        // 如果因为异常而退出循环
        if (failed)
            cancelAcquire(node);
    }
}

// 返回节点的前驱
final Node predecessor() throws NullPointerException {
    Node p = prev;
    if (p == null)
        throw new NullPointerException();
    else
        return p;
}

private static boolean shouldParkAfterFailedAcquire(Node pred, Node node) {
    // 前驱节点的状态
    int ws = pred.waitStatus;
    
    // 表示后边的节点对应的线程处于等待状态
    // 设置状态正确
    if (ws == Node.SIGNAL)
        /*
             * This node has already set status asking a release
             * to signal it, so it can safely park.
             */
        return true;
    
    // ws == 1
    // 前驱节点放弃等待
    if (ws > 0) {
        /*
             * Predecessor was cancelled. Skip over predecessors and
             * indicate retry.
             */
        do {
        	// pred = 前驱节点的前驱
            // 当前节点的前驱 = 前驱节点的前驱
            node.prev = pred = pred.prev;
            // 一直找到没有被取消的一个前驱节点
        } while (pred.waitStatus > 0);
        // 这个没有被取消的前驱节点 后继 设为当前节点
        pred.next = node;
    } else {
        /*
             * waitStatus must be 0 or PROPAGATE.  Indicate that we
             * need a signal, but don't park yet.  Caller will need to
             * retry to make sure it cannot acquire before parking.
             */
        // 前驱节点没有放弃等待, 但是状态不对
        // 那么 设置为正确状态
        compareAndSetWaitStatus(pred, ws, Node.SIGNAL);
    }
    return false;
}

private final boolean parkAndCheckInterrupt() {
    LockSupport.park(this);
    return Thread.interrupted();
}


private void cancelAcquire(Node node) {
    // Ignore if node doesn't exist
    if (node == null)
        return;

    node.thread = null;

    // Skip cancelled predecessors
    Node pred = node.prev;
    while (pred.waitStatus > 0)
        node.prev = pred = pred.prev;

    // predNext is the apparent node to unsplice. CASes below will
    // fail if not, in which case, we lost race vs another cancel
    // or signal, so no further action is necessary.
    Node predNext = pred.next;

    // Can use unconditional write instead of CAS here.
    // After this atomic step, other Nodes can skip past us.
    // Before, we are free of interference from other threads.
    node.waitStatus = Node.CANCELLED;

    // If we are the tail, remove ourselves.
    if (node == tail && compareAndSetTail(node, pred)) {
        compareAndSetNext(pred, predNext, null);
    } else {
        // If successor needs signal, try to set pred's next-link
        // so it will get one. Otherwise wake it up to propagate.
        int ws;
        if (pred != head &&
            ((ws = pred.waitStatus) == Node.SIGNAL ||
             (ws <= 0 && compareAndSetWaitStatus(pred, ws, Node.SIGNAL))) &&
            pred.thread != null) {
            Node next = node.next;
            if (next != null && next.waitStatus <= 0)
                compareAndSetNext(pred, predNext, next);
        } else {
            unparkSuccessor(node);
        }

        node.next = node; // help GC
    }
}
```



释放锁

```java
public final boolean release(int arg) {
    // 如果可以释放锁啦
    if (tryRelease(arg)) {
        Node h = head;
        // 队列不为空
        // 当前没有执行唤醒操作
        if (h != null && h.waitStatus != 0)
            // 唤醒被挂起的线程
            unparkSuccessor(h);
        return true;
    }
    return false;
}

private void unparkSuccessor(Node node) {
    /*
         * If status is negative (i.e., possibly needing signal) try
         * to clear in anticipation of signalling.  It is OK if this
         * fails or if status is changed by waiting thread.
         */
    // 节点的状态
    int ws = node.waitStatus;
    // 设置0号节点状态为0，表示正在唤醒一个线程
    // 其他唤醒操作请走开
    if (ws < 0)
        compareAndSetWaitStatus(node, ws, 0);

    /*
         * Thread to unpark is held in successor, which is normally
         * just the next node.  But if cancelled or apparently null,
         * traverse backwards from tail to find the actual
         * non-cancelled successor.
         */ 
    // s 是 1号节点
    Node s = node.next;
    // 如果 1号节点为空
    // 或者 1号节点取消了
    if (s == null || s.waitStatus > 0) {
        s = null;
        // 从队列尾部开始遍历
        for (Node t = tail; t != null && t != node; t = t.prev)
            // 一直找到第一个没有被取消的节点
            if (t.waitStatus <= 0)
                s = t;
    }
    // 如果找到了 第一个没有被取消的节点
    if (s != null)
        // 唤醒该线程
        LockSupport.unpark(s.thread);
}
```

