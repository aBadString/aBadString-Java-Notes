<h1 align="center">Java 并发编程</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. volatile](#1-volatile)
  - [1.1. volatile 是什么](#11-volatile-是什么)
  - [1.2. JMM Java内存模型](#12-jmm-java内存模型)
  - [1.3. 双重锁定检查的单例模式](#13-双重锁定检查的单例模式)
    - [1.3.1. 对象的发布和逸出](#131-对象的发布和逸出)
- [2. CAS 比较并交换](#2-cas-比较并交换)
- [3. 集合类的线程不安全情况](#3-集合类的线程不安全情况)
  - [3.1. 并发修改异常 java.util.ConcurrentModificationException](#31-并发修改异常-javautilconcurrentmodificationexception)
    - [3.1.1. 异常现象](#311-异常现象)
    - [3.1.2. 异常原因](#312-异常原因)
    - [3.1.3. 解决方案](#313-解决方案)
    - [3.1.4. CopyOnWriteArrayList](#314-copyonwritearraylist)
  - [3.2. 其它集合类](#32-其它集合类)
    - [3.2.1. ConcurrentHashMap](#321-concurrenthashmap)
  - [3.3. 同步容器](#33-同步容器)
  - [3.4. 并发容器](#34-并发容器)
- [4. 锁](#4-锁)
  - [4.1. AbustactQueuedSynchronizer](#41-abustactqueuedsynchronizer)
    - [4.1.1. 同步状态](#411-同步状态)
    - [4.1.2. 同步队列](#412-同步队列)
    - [4.1.3. 源码分析](#413-源码分析)
  - [4.2. 公平锁和非公平锁](#42-公平锁和非公平锁)
  - [4.3. 可重入锁（递归锁）](#43-可重入锁递归锁)
    - [4.3.1. ReentrantLook 源码](#431-reentrantlook-源码)
  - [4.4. 自旋锁（Unsafe类 CAS）](#44-自旋锁unsafe类-cas)
  - [4.5. 独占锁(写锁、排他锁)和共享锁(读锁)](#45-独占锁写锁-排他锁和共享锁读锁)
- [5. 线程工具类](#5-线程工具类)
  - [5.1. CountDownLatch](#51-countdownlatch)
  - [5.2. CyclicBarrier](#52-cyclicbarrier)
  - [5.3. Semaphore 信号量](#53-semaphore-信号量)
- [6. 阻塞队列](#6-阻塞队列)
  - [6.1. BlockingQueue 核心方法](#61-blockingqueue-核心方法)
  - [6.2. 再探生产者消费者模型](#62-再探生产者消费者模型)

<!-- /code_chunk_output -->

# 1. volatile

## 1.1. volatile 是什么

volatile 是 Java 虚拟机提供的轻量级同步机制。
- 保证该变量对所有线程可见。确保变量的更新以可预见的方式告知其他的线程。(volatile 变量不会缓存在寄存器或者缓存在其他处理器隐藏的地方)
- 禁止指令重排序优化。它是共享的，对它的操作不会与其他内存操作一起被重排序。
- 不保证原子性。

volatile 只遵守了 JMM 的两个规范：可见性、有序性；不遵守原子性。

要使用 volatile 变量，得满足：
1. 写入变量时并不依赖变量的当前值；或者能够确保只有单一的线程修改变量的值
2. 变量不需要与其他状态变量参与不变约束
3. 访问变量时，没有其他的原因需要加锁

注：没有被声明位 volatile 的 64 位数值变量 (double、long)，JVM 允许将64位的读写划分为两个 32 位操作。

## 1.2. JMM Java内存模型

Java Memory Model 是一个抽象的存在，并不实际存在。
JMM关于同步有如下规定：
- 线程加锁之前，必须读取主内存中最新的值到自己的工作内存
- 线程解锁之前，必须把共享变量的值刷新回主内存
- 加锁和解锁操作是针对同一把锁

工作内存：对于Java的每个线程，都会为其创建一个私有的工作内存。
主内存：所有的共享变量都存储在主内存中，主内存是共享的，所有线程都可以访问。
线程对共享变量的操作只能在工作内存中进行操作，所以要先将主内存中的共享变量拷贝到自己的工作内存中去，操作完成后再把变量的值更新到主内存。

JMM 三大特性：
可见性：当一个线程对共享变量修改，并更新到主内存中了之后，其他线程必须第一时间知道该共享变量更新了。
原子性：不可分割，完整性。线程中某个操作作为一个整体，要么全部完成，要么全部失败。
有序性：禁止指令重排序优化

**指令重排序**：为了提高性能，编译器和处理器常常会对指令进行重新排序，分三种：
```
源代码 -> 编译器优化的重排 -> 指令并行的重排 -> 内存系统的重排 -> 最终执行的指令
```
在单线程的环境下，重排序后的代码执行结果和源代码执行结果相同。
在进行指令重排时，要考虑指令之间的数据依赖性。（先行发生原则）

**先行发生原则**：如果说操作A先行发生于操作B，则说明操作A产生的影响能够被B观察到。如果两个操作之间没有先行关系，虚拟机可以对它们进行随意的重排序。
先行发生原则：
- 程序次序规则：源代码中书写在前面的代码 先行发生于 后面的。
- 管程锁定规则：unlock 操作 先行发生于 之后对于同一个锁的 lock 操作。只有释放了锁之后，才能加锁。
- volatile 变量规则：同一个 volatile 变量的写操作 先行发生于 之后的读操作。
- 线程启动规则：Thread 对象的 start 方法 先行发生于 此线程的每一个动作。
- 线程终止规则：线程的每一个动作 先行发生于 线程终止操作。
- 线程中断规则：对interrupt 方法的调用 先行发生于 线程检测到中断的发生
- 对象终结规则：一个对象的构造器执行 先行发生于 finalize 方法的执行
- 传递性：A 先行发生于 B、B 先行发生于 C，那么 A 先行发生于 C

**内存屏障**：是一条CPU指令，它的作用是：
1. 保证特定操作的执行顺序
2. 保证某些变量的内存可见性
通过插入内存屏障指令，禁止在内存屏障前后的指令进行重排序。
![](../images/Java高级特性/内存屏障.png)


可见性例子：
下面代码如果没有volatile修饰num，则main线程永远不会退出。
```java
class MyData {
    volatile int num;
}
public class Visibility {
    public static void main(String[] args) {
        MyData myData = new MyData();

        new Thread(() -> {
            System.out.println(Thread.currentThread().getName() + "进入");
            // 阻塞 3 秒
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            // 修改 num 的值
            myData.num = 60;
            System.out.println(Thread.currentThread().getName() + "将myData.num修改为" + myData.num);
        }, "线程A").start();

        while (myData.num == 0) {
            continue;
        }
        System.out.println("mian 线程发现了myData.num修改为" + myData.num);
    }
}
// 线程A进入
// 线程A将myData.num修改为60
// mian 线程发现了myData.num修改为60
```

不保证原子性的例子
不能保证计算结果为 20000
```java
class MyData {
    volatile int num;
}
public class Atomicity {
    public static void main(String[] args) {
        MyData myData = new MyData();

        for (int i = 1; i <= 20; i++) {
            new Thread(()->{
                for (int j = 0; j < 1000; j++) {
                    myData.num++;
                }
            }, String.valueOf(i)).start();
        }

        // 有两个线程：main 线程 和 gc 线程
        while (Thread.activeCount() > 2) {
            Thread.yield();
        }
        System.out.println(myData.num);
    }
}
// 结果：
// 19987
```

反编译 i++ 字节码指令
```java
class MyData {
    volatile int num = 0;
    void add() {
        num++;
    }
}
```
```class
class atguigu.volatiledemo.MyData {
  volatile int num;

  atguigu.volatiledemo.MyData();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: aload_0
       5: iconst_0
       6: putfield      #2                  // Field num:I
       9: return

  void add();
    Code:
       0: aload_0           // 把局部变量表0号位（this）复制到操作数栈
       1: dup               // 复制栈顶部一个字长内容
       2: getfield #2                  // Field num:I
       5: iconst_1
       6: iadd
       7: putfield #2                  // Field num:I
      10: return
}

============================================================
// num++ 被拆分为了三个指令
// 1、执行 getfield 拿到 num 原值
// 2、执行 iadd 进行加 1 操作
// 3、执行 putfield 将值写回

    2: getfield      #2                  // Field num:I
    5: iconst_1
    6: iadd
    7: putfield      #2                  // Field num:I
============================================================
```

如何解决原子性问题：
- synchronized
- java.util.concurrent.atomic.AtomicInteger

原子类例子
AtomicInteger 不需要 volatile 修饰，因为它实际值存放的 value 是用 volatile 修饰的 `private volatile int value;`
```java
class MyData {
    volatile int num = 0;
    AtomicInteger atomNum = new AtomicInteger();
}
public class Atomicity {
    public static void main(String[] args) {
        MyData myData = new MyData();

        for (int i = 1; i <= 20; i++) {
            new Thread(()->{
                for (int j = 0; j < 10000; j++) {
                    myData.num++;
                    myData.atomNum.getAndIncrement();
                }
            }, String.valueOf(i)).start();
        }

        // 有两个线程：main 线程 和 gc 线程
        while (Thread.activeCount() > 2) {
            Thread.yield();
        }
        System.out.println(myData.num);
        System.out.println(myData.atomNum);
    }
}
// 175810
// 200000
```

## 1.3. 双重锁定检查的单例模式

```java
class Singleton {
    // 1.私有化构造器
    private Singleton() {}
    // 2.类内部创建一个本类的对象
    private static Singleton t = null;
    // 3.提供一个方法来获取实例对象
    public static Singleton getInstance() {
        if (t == null) {
            synchronized (Singleton.class) {  
                if (t == null) {  
                    t = new Singleton(); 
                }  
            }
        }
        return t;
    }
}
```
不一定安全，存在指令重排序的可能。

t = new Singleton(); 不是原子操作，并且步骤 2 3 允许重排
1. memory = allocate(); // 分配对象内存空间
2. init(memory); // 初始化对象
3. t = memory; 赋值操作，此时 t != null

如果重排后是 1 3 2，会导致还没有初始化完成的对象的引用 t 被 return 出去。造成**对象逸出**。 
解决方法：给 t 加上 volatile 禁止指令重排： `private static volatile Singleton t = null;` 

### 1.3.1. 对象的发布和逸出
发布一个对象，是使它能够被当前范围之外的代码所使用。
一个对象在尚未准备好时就将其发布，这称为逸出


# 2. CAS 比较并交换

CAS 是一条 CPU 并发原语，是一条原子指令。
这个指令需要三个操作数：内存位置 V，旧的预期值 A，新值 B。
当且仅当 V 的值 符合 A 时，CPU 用 B 更新 V 的值；否则不更新。但都会返回 V 的旧值。
J.U.C 包中的整数原子类即使用了 CAS 操作

```java
public class AtomicInteger extends Number implements java.io.Serializable {
    private static final long serialVersionUID = 6214790243416807050L;
    // setup to use Unsafe.compareAndSwapInt for updates
    // Unsafe 类进行CAS操作
    private static final Unsafe unsafe = Unsafe.getUnsafe();
    private static final long valueOffset;

    static {
        try {
            valueOffset = unsafe.objectFieldOffset
                (AtomicInteger.class.getDeclaredField("value"));
        } catch (Exception ex) { throw new Error(ex); }
    }
    // volatile 保证对原子类值的修改的可见性
    private volatile int value;
```

**Unsafe类**
sum.misc.Unsafe 类是CAS核心类，存在于 rt.jar 中。其内部方法可以想 C 指针一样直接操作内存。
```java
// AtomicInteger的++操作依赖于Unsafe的getAndAddInt
public final int getAndIncrement() {
    return unsafe.getAndAddInt(this, valueOffset, 1);
}

// Unsafe的getAndAddInt
public final int getAndAddInt(Object o, long offset, int delta) {
    int v;
    do {
        // 获取工作内存中的最新值
        v = getIntVolatile(o, offset); // o: 626627285 offset: 12
    } while (!compareAndSwapInt(o, offset, v, v + delta)); // CAS 操作，如果失败：循环重新拿最新值
    return v;
}
```
上述代码的伪代码流程
```java
do {
    // 获取主内存中的值
    int A = getValue(V); V = o(对象) + offset(偏移地址);
    // 进行操作
    int B = A + 1;
    // CAS 操作，将新值 B 写回主内存。如果失败返回 false，重新拿主内存中的最新值，进行操作，在写回。
} while (!CAS(V, A, B));
```
这个操作就很像git推送分支到远程仓库上：拉去最新的分支内容，然后写 bug，写完之后尝试推送分支；如果推送失败，则拉取远程分支，解决冲突，再推送。

**缺点：**
1. 循环时间长，开销大，占用 CPU 却不干活。
2. 只能保证**一个**共享变量的原子操作。不能保证多个变量的原子性。
2. 可能会发生ABA问题。

**ABA 问题**

当一个线程 t1 从内存位置 V 取出值 A，进行操作。再 t1 操作过程中，另一个线程 t2 将 V 处的值改为了 B，之后 t2 又将值改回了 A。此时线程 t1 操作完成，执行 CAS 原语将其操作结果写回 V 处，操作成功。
也就是说线程 t1 对线程 t2 执行的操作是不知情的。

原子引用是会发生ABA问题的：
```java
public class ABA {
    public static void main(String[] args) {
        AtomicReference<Integer> ref = new AtomicReference<>(100);

        Thread t2 = new Thread(() -> {
            Integer A = ref.get();
            System.out.println(Thread.currentThread().getName()+": " + A);

            ref.compareAndSet(A, 101);
            A = ref.get();
            System.out.println(Thread.currentThread().getName()+": " + A);

            ref.compareAndSet(A, 100);
            A = ref.get();
            System.out.println(Thread.currentThread().getName()+": " + A);
        }, "t2");


        new Thread(()->{
            Integer A = ref.get();
            System.out.println(Thread.currentThread().getName()+": " + A);

            t2.start();
            try {
                // 线程t1等待线程t2执行完成
                t2.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            boolean flag = ref.compareAndSet(A, 102);
            System.out.println(flag);
            System.out.println(Thread.currentThread().getName()+": " + ref.get());
        }, "t1").start();
    }
}
// t1: 100
// t2: 100 // A
// t2: 101 // B
// t2: 100 // A
// true    // 线程 t1 并不知道线程 t2 修改过 A 的值
// t1: 102
```

解决方法：AtomicStampedReference，修改数据后更新版本号
```java
public class AtomicStampedReferenceDemo {
    public static void main(String[] args) {
        AtomicStampedReference<Integer> ref = new AtomicStampedReference<>(100, 1);
        Thread t2 = new Thread(() -> {
            int stamp = ref.getStamp();
            Integer A = ref.getReference();
            System.out.println(Thread.currentThread().getName()+": " + A + "\t" + stamp);

            ref.compareAndSet(A, 101, stamp, stamp+1);
            stamp = ref.getStamp();
            A = ref.getReference();
            System.out.println(Thread.currentThread().getName()+": " + A + "\t" + stamp);

            ref.compareAndSet(A, 100, stamp, stamp+1);
            stamp = ref.getStamp();
            A = ref.getReference();
            System.out.println(Thread.currentThread().getName()+": " + A + "\t" + stamp);
        }, "t2");

        new Thread(()->{
            int stamp = ref.getStamp();
            Integer A = ref.getReference();
            System.out.println(Thread.currentThread().getName()+": " + A + "\t" + stamp);

            t2.start();
            try {
                // 线程t1等待线程t2执行完成
                t2.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            boolean flag = ref.compareAndSet(A, 100, stamp, stamp+1);
            System.out.println(flag);
            stamp = ref.getStamp();
            A = ref.getReference();
            System.out.println(Thread.currentThread().getName()+": " + A + "\t" + stamp);
        }, "t1").start();
    }
}
// t1: 100	1
// t2: 100	1  // A
// t2: 101	2  // B
// t2: 100	3  // A
// false       // 线程 t1 发现版本号不对，就知道有线程修改过 A 的值
// t1: 100	3
```

# 3. 集合类的线程不安全情况

## 3.1. 并发修改异常 java.util.ConcurrentModificationException

### 3.1.1. 异常现象

下面的程序，由于三个线程同时使用一个 list，会导致出现一些错误的情形。但还没有抛出异常
```java
public static void main(String[] args) {
    List<String> list = new ArrayList<>();

    for (int i = 1; i <= 30; i++) {
        new Thread(() -> {
            String s = UUID.randomUUID().toString().substring(0, 8);
            list.add(s);
            System.out.println(Thread.currentThread().getName() + " add " + s);
        }, String.valueOf(i)).start();
    }

    while (Thread.activeCount() > 2) {
        Thread.yield();
    }
    System.out.println(list.size());
    System.out.println(list);
}
// 1 add 7e742601
// 3 add 77548695
// 2 add 2c5f0ed3
// 3
// [7e742601, 2c5f0ed3, 77548695]

// 2 add 6aa654d0
// 1 add c09d441b
// 3 add 44605ac5
// 3
// [null, 6aa654d0, c09d441b]

// 1 add 83ee8584
// 2 add bc415695
// 3 add 38d473e6
// 2
// [bc415695, 38d473e6]

// 3 add 1a603579
// 2 add 0ad81e3b
// 1 add af204932
// 1
// [0ad81e3b]
```
但是当我们试图去读取list列表时，就会发生并发修改异常 java.util.ConcurrentModificationException
```java
public static void main(String[] args) {
        List<String> list = new ArrayList<>();

        for (int i = 1; i <= 30; i++) {
            new Thread(() -> {
                String s = UUID.randomUUID().toString().substring(0, 8);
                list.add(s);
                // 读取list的全部元素
                list.forEach(System.out::println);
                System.out.println(Thread.currentThread().getName() + " add " + s);
            }, String.valueOf(i)).start();
        }

        while (Thread.activeCount() > 2) {
            Thread.yield();
        }
        System.out.println(list.size());
        System.out.println(list);
    }
```

### 3.1.2. 异常原因

[Java 基础编程 · 17.5. 集合有关面试题](./01_Java基础编程.md#175-集合有关面试题)

### 3.1.3. 解决方案

1. 使用 Vector 类（不建议，锁方法，导致并发现急剧下降）
2. Collections.synchronizedList(new ArrayList<>());
3. CopyOnWriteArrayList

### 3.1.4. CopyOnWriteArrayList
![image-20200326192142191](../images/image-20200326192142191.png)

CopyOnWriteArrayList add 方法：写时复制，当要往list中添加元素时，不会在原list上直接修改，而是拷贝一份原list，在拷贝的list上做修改；修改完成之后用拷贝的数组更新原来的引用。
```java
public boolean add(E e) {
    // 轻量级锁
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        Object[] elements = getArray();
        int len = elements.length;
        // 复制
        Object[] newElements = Arrays.copyOf(elements, len + 1);
        // 写
        newElements[len] = e;
        // 更新最新的数组
        setArray(newElements); // array = newElements;
        return true;
    } finally {
        lock.unlock();
    }
}
```


## 3.2. 其它集合类

HashSet 也会出现并发修改异常。
1. Collections.synchronizedSet(new HashSet<>())
2. CopyOnWriteArraySet
CopyOnWriteArraySet 依赖的就是一个 CopyOnWriteArrayList（这 Set 家族这么不要脸么。HashSet 套壳 HashMap，真省事）
```java
public class CopyOnWriteArraySet<E> extends AbstractSet<E>
        implements java.io.Serializable {
    private static final long serialVersionUID = 5457747651344034263L;

    private final CopyOnWriteArrayList<E> al;

    /**
     * Creates an empty set.
     */
    public CopyOnWriteArraySet() {
        al = new CopyOnWriteArrayList<E>();
    }
}
```

HashMap 也会出现并发修改异常。
1. HashTable （不推荐，锁方法，导致并发现急剧下降）
1. Collections.synchronizedMap(new HashMap<>())
2. ConcurrentHashMap

### 3.2.1. ConcurrentHashMap

- 线程安全，并且锁分离。
- 使用 **分段锁**，降低了锁粒度，让并发度大大提高，允许更深层次的共享访问。ConcurrentHashMap内部使用段(Segment)来表示这些不同的部分，每个段其实就是一个小的HashTable，它们有自己的锁。
- 任意数量的读线程可以并发访问；读者和写者也可以并发访问；有限数量的写线程可以并发修改（只要多个修改操作发生在不同的段上，它们就可以并发进行）。

好处：
- 为并发访问带来更高的吞吐量，并且几乎没有损失单个线程访问的性能。
- 其迭代器不会抛出 ConcurrentModificationException  异常，不需要在迭代中加锁。返回的迭代器具有 **弱一致性**，可以感知 (但不保证) 在迭代器被创建后容器的修改。

**jdk 1.8 源码**
```java
/**
 * Creates a new, empty map with the default initial table size (16).
 * 使用默认的初始表大小（16）创建一个新的空Map。
 */
public ConcurrentHashMap() {
}
```
构造时不会初始化数组，第一次加入元素时才初始化
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


## 3.3. 同步容器

同步容器类包括：
1. Vector 和 HashTable
2. jdk 1.2 加入的 同步包装类 (wrapper)
3. 由 Collections.synchronizedXxx 工厂方法创建

这些类通过封装它们的状态，并对每个公共方法进行同步，来实现线程安全。
会出现的问题：对于复合操作，需要额外的加锁处理。
迭代器 ConcurrentModificationException 并发修改异常

```java
public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m) {
    return new SynchronizedMap<>(m);
}
// SynchronizedMap类是一个静态内部类。
// SynchronizedMap的每个方法，都是对Map本身做了一次带锁的操作。该类本身并没有太多的应用，性能较差。
private static class SynchronizedMap<K,V> implements Map<K,V>, Serializable {
    private final Map<K,V> m;     // Backing Map
    final Object mutex;           // Object on which to synchronize

    public V put(K key, V value) {
        synchronized (mutex) {return m.put(key, value);}
    }
    public V remove(Object key) {
        synchronized (mutex) {return m.remove(key);}
    }
    public void putAll(Map<? extends K, ? extends V> map) {
        synchronized (mutex) {m.putAll(map);}
    }
    public void clear() {
        synchronized (mutex) {m.clear();}
    }
}
```

## 3.4. 并发容器

jdk 5 提供了几种并发的容器类。
ConcurrentHashMap、CopyOnWriteArrayList、CopyOnWriteArraySet
用并发容器替换同步容器，这样以很小的风险带来并发性的显著提高。


# 4. 锁

## 4.1. AbustactQueuedSynchronizer

AbustactQueuedSynchronizer 是J ava 提供的底层同步工具类，用一个int类型的变量表示同步状态，并提供了一系列的 CAS 操作来管理这个同步状态。

### 4.1.1. 同步状态

AQS 维护了一个 int 类型的变量来表示同步状态。
```java
// 同步状态
private volatile int state;
```
对 state 可以进行的操作，由 AQS 实现，提供给我们重写 AQS 5 大方法的基础操作。
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

基于 AQS 的锁示例：
```java
public class PlainLock {
    private static class Sync extends AbstractQueuedSynchronizer {
        @Override
        protected boolean tryAcquire(int arg) {
            return compareAndSetState(0, 1);
        }
        @Override
        protected boolean tryRelease(int arg) {
            setState(0);
            return true;
        }
        @Override
        protected boolean isHeldExclusively() {
            return getState() == 1;
        }
    }

    private Sync sync = new Sync();

    public void lock() {
        sync.acquire(1);
    }

    public void unlock() {
        sync.release(1);
    }
}
```

### 4.1.2. 同步队列

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
【我猜的】因为队列前面的线程等待时间都比较长，很大可能已经取消等待了。从前往后的话需要遍历次数较多。


### 4.1.3. 源码分析

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

## 4.2. 公平锁和非公平锁

公平锁：线程按照申请锁的顺序来获取锁。
非公平锁：并不严格按照或者完全不按照申请锁的顺序来获取锁，后申请锁的线程可以先获得锁。
在高并发的情况下，可能会造成获取锁的顺序反转而造成饥饿现象。

饥饿现象：某个线程一直被插队，导致长时间获取不到锁。

公平锁一定会去查看阻塞队列是否为空，如果不为空则排队；为空则尝试获取锁。而非公平锁会直接去尝试获取锁，如果获取失败，才去排队。
非公平锁的好处：如果一个线程释放锁之后，队列中还有线程在等待，另一个线程马上来获取锁，然后进入临界区。这样减少了唤醒队列中线程和阻塞刚来的线程的开销，提高了CPU的利用率，提高了线程并发效率。

结合国赛出租车蓄车池：队列中走 N 个，刚来的走 1 个。

## 4.3. 可重入锁（递归锁）

同一个线程可以多次进入已经获取的同一个可重入锁的临界区。
作用：避免死锁。
可重入说明：锁的请求是基于每线程的，而不是每调用。

例如：一个线程获取当前this对象锁进入 method01，之后又进入 method02，此时自动获取锁。也就是说这个 this 锁可以被同一个线程获取多次。
```java
void synchronized method01() {
    method02()
}
void synchronized method02() {
    
}
```
synchronized和ReentrantLook都是可重入锁

ReentrantLook 可以对一段代码块加多次锁，但要注意加锁几次就要解锁几次。如果少解锁一次，导致锁一直不被释放，后面要获取锁的线程就会陷入死锁状态。
```java
class DeadLock implements Runnable {
    Lock lock = new ReentrantLock();

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "开始执行");
        lock.lock();
        lock.lock();
        System.out.println(Thread.currentThread().getName() + "获取锁，执行方法");
        lock.unlock();
        System.out.println(Thread.currentThread().getName() + "结束执行");
    }
}
public class ReentrantLookDemo {
    public static void main(String[] args) {
        DeadLock deadLock = new DeadLock();
        new Thread(deadLock, "t1").start();
        new Thread(deadLock, "t2").start();
    }
}
```
如果多解锁一次，则会抛出非法监视器状态异常 java.lang.IllegalMonitorStateException


### 4.3.1. ReentrantLook 源码

ReentrantLook 依赖 AQS 来实现，有的公平锁和非公平锁

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
                !hasQueuedPredecessors() // 如果队列种有线程等待,返回false。这里体现了公平性
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
从源码可以看出，无论是公平锁还是非公平锁。都是使用了 AQS 的独占模式。所以 ReentrantLook 是独占锁。


## 4.4. 自旋锁（Unsafe类 CAS）

自旋锁：一个线程如果获取自旋锁失败，它不会立即被阻塞，而是进入一段盲循环代码，不断循环获取锁。
好处：减少线程上下文的切换。阻塞线程是一个特权指令，需要系统转换到内核态。 
缺点：占用消耗 CPU。

Java 线程的实现：Java 线程是虚拟机中的实体，最终还是要落地到操作系统上的。Java 线程实现有三种：
1. 使用内核线程实现：内核线程就是直接由操作系统内核支持的线程，这种线程由内核来进行线程调度。一般使用轻量级进程。这种需要使用系统调用，要切换到内核态。
2. 使用用户线程实现：不需要切换到内核态，所以线程操作由用户程序自己处理。难以实现，逐渐放弃。
3. 使用用户线程加轻量级进程混合实现

JDK1.2之前使用用户线程实现；之后基于操作系统原生线程来实现。不同 JVM 实现不同。

自旋锁实现：
```java
class SpinLock {
    private AtomicReference<Thread> cur = new AtomicReference<Thread>();

    public void lock() {
        while (!cur.compareAndSet(null, Thread.currentThread())) {

        }
    }
    public void unlock() {
        cur.compareAndSet(Thread.currentThread(), null);
    }
}

public class SpinLockDemo {
    private static volatile int num;

    public static void main(String[] args) {
        SpinLock lock = new SpinLock();

        for (int i = 1; i <= 20; i++) {
            new Thread(()->{
                for (int j = 0; j < 10000; j++) {
                    lock.lock();
                    num++;
                    lock.unlock();
                }
            }, String.valueOf(i)).start();
        }

        // 有两个线程：main 线程 和 gc 线程
        while (Thread.activeCount() > 2) {
            Thread.yield();
        }
        System.out.println(num);
    }
}
// 200000
```

## 4.5. 独占锁(写锁、排他锁)和共享锁(读锁)

独占锁：一次只能同时被一个线程所拥有，其他线程不可再获取到该锁。synchronized 和 ReentrantLook都是独占锁
共享锁：可以同时被多个线程所拥有。ReentrantReadWriteLock 的读锁是共享锁，写锁是独占锁。

读锁的共享性可以保证高效的并发读操作；读写和写写操作是互斥的。

ReadWriteLock 是一个读写锁的接口，ReentrantReadWriteLock 是它的一个具体实现类，依赖 AQS 的独占模式和共享模式实现读写锁。
```java
package java.util.concurrent.locks;
public interface ReadWriteLock {
    /**
     * Returns the lock used for reading.
     * @return the lock used for reading
     */
    Lock readLock();

    /**
     * Returns the lock used for writing.
     * @return the lock used for writing
     */
    Lock writeLock();
}

public class ReentrantReadWriteLock implements ReadWriteLock, java.io.Serializable {
    private static final long serialVersionUID = -6992448646407690164L;
    /** 读锁 */
    private final ReentrantReadWriteLock.ReadLock readerLock;
    /** 写锁*/
    private final ReentrantReadWriteLock.WriteLock writerLock;
    /** Performs all synchronization mechanics */
    final Sync sync;

    // 默认非公平锁
    public ReentrantReadWriteLock() {
        this(false);
    }
    // 可选择公平与否
    public ReentrantReadWriteLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();
        readerLock = new ReadLock(this);
        writerLock = new WriteLock(this);
    }
    // 实现 ReadWriteLock 接口，对外提供两种锁的实例
    public ReentrantReadWriteLock.WriteLock writeLock() { return writerLock; }
    public ReentrantReadWriteLock.ReadLock  readLock()  { return readerLock; }

    // 这个类真正实现了 AQS 的 5 大方法
    // FairSync 和 NonfairSync 是 Sync 的子类：对获取锁的判断做了公平和不公平的操作
    abstract static class Sync extends AbstractQueuedSynchronizer {

    }
    // 读锁
    public static class ReadLock implements Lock, java.io.Serializable {
        private static final long serialVersionUID = -5992448646407690164L;
        private final Sync sync;
        protected ReadLock(ReentrantReadWriteLock lock) {
            sync = lock.sync;
        }
        // 读锁是共享锁，加锁时实现的是 AQS 的 acquireShared 方法。即共享模式
        public void lock() {
            sync.acquireShared(1);
        }
        public void unlock() {
            sync.releaseShared(1);
        }
    }
    // 写锁
    public static class WriteLock implements Lock, java.io.Serializable {
        private static final long serialVersionUID = -4992448646407690164L;
        private final Sync sync;
        protected WriteLock(ReentrantReadWriteLock lock) {
            sync = lock.sync;
        }
        // 写锁是独占锁，加锁时实现的是 AQS 的 acquire 方法。即独占模式
        public void lock() {
            sync.acquire(1);
        }
        public void unlock() {
            sync.release(1);
        }
    }
}
```

# 5. 线程工具类

## 5.1. CountDownLatch
被 await 的线程需要等待前面 count 个线程完成，才继续进行。
```java
// 构造器指明 count
public CountDownLatch(int count) {
    if (count < 0) throw new IllegalArgumentException("count < 0");
    this.sync = new Sync(count);
}
// 使 count 减一
public void countDown() {
    sync.releaseShared(1);
}
// 线程自旋，知道 count 为 0
public void await() throws InterruptedException {
    sync.acquireSharedInterruptibly(1);
}
```
例子：
```java
public class CountDownLatchDemo {
    public static void main(String[] args) {
        CountDownLatch latch = new CountDownLatch(6);

        for (int i = 1; i <= 6; i++) {
            new Thread(()->{
                System.out.println(Thread.currentThread().getName() + "线程完成工作");
                latch.countDown();
            }, String.valueOf(i)).start();
        }

        System.out.println("主线程等待其他线程完成工作");
        try {
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("主线程等待完毕");
    }
}
// 主线程等待其他线程完成工作
// 1线程完成工作
// 2线程完成工作
// 3线程完成工作
// 4线程完成工作
// 5线程完成工作
// 6线程完成工作
// 主线程等待完毕
```

## 5.2. CyclicBarrier
让一组线程到达一个屏障（同步点）时被阻塞，直到所有线程都到齐时才放行。
```java
public class CyclicBarrierDemo {
    public static void main(String[] args) {
        CyclicBarrier barrier = new CyclicBarrier(4, () -> {
            System.out.println("线程到齐");
        });

        for (int i = 1; i <= 4; i++) {
            new Thread(()->{
                System.out.println(Thread.currentThread().getName() + " 到达");
                try {
                    barrier.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + " 继续执行");
            }, String.valueOf(i)).start();
        }
    }
}
// 1 到达
// 3 到达
// 2 到达
// 4 到达
// 线程到齐
// 4 继续执行
// 3 继续执行
// 2 继续执行
// 1 继续执行
```

## 5.3. Semaphore 信号量

作用：
1. 用于多个共享资源的互斥使用
2. 用于并发线程数量的控制

```java
public class SemaphoreDemo {
    public static void main(String[] args) {
        Semaphore semaphore = new Semaphore(3);
        for (int i = 1; i <= 6; i++) {
            new Thread(()->{
                try {
                    semaphore.acquire();
                    System.out.println(Thread.currentThread().getName() + "进入临界区");
                    try {
                        TimeUnit.SECONDS.sleep(3);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + "离开临界区");
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release();
                }
            }, String.valueOf(i)).start();
        }
    }
}
// 1进入临界区
// 3进入临界区
// 2进入临界区
// 3离开临界区
// 2离开临界区
// 1离开临界区
// 5进入临界区
// 4进入临界区
// 6进入临界区
// 5离开临界区
// 6离开临界区
// 4离开临界区
```

# 6. 阻塞队列

1. **ArrayBlockingQueue**：有界队列，用数组实现的，FIFO
2. **LinkedBlockingQueue**：有界队列，基于链表，FIFO。可设置容量队列，不设置容量则最大为 Integer.MAX_VALUE
3. PriorityBlockingQueue：优先级队列
4. DelayQueue：延迟无界队列，其中的对象到期时才能从队列中取走，使用优先级队列实现
5. **SynchronousQueue**：同步队列，只有单个元素的队列。插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态
6. LinkedTransferQueue：无界队列，基于链表
7. LinkedBlockingDeque：双向队列，基于链表

## 6.1. BlockingQueue 核心方法

| 方法类型 | 抛出异常 | 返回特殊值 | 阻塞   | 超时间              |
|:-------:|:--------:|:---------:|:------:|:------------------:|
| 插入    | add(e)    | offer(e) | put(e) | offer(e,time,unit) |
| 移除    | remove()  | poll()   | take() | poll(time,unit)    |
| 检查    | element() | peek()   | ——     | ——                 |

抛出异常：
当队列满了，add 插入元素会抛出异常 IllegalStateException:Queue full;
当队列为空时，element 查看队首元素和 remove 移除元素会抛出 NoSuchElementException

返回特殊值：
当队列满了，offer 插入元素返回 false
当队列为空时，poll 移除元素会返回 null

阻塞：
put 和 take 在插入元素或移除元素失败时会阻塞线程，直到可以进行操作时唤醒

## 6.2. 再探生产者消费者模型

线程操作资源类，高内聚低耦合
判断 干活 通知
防止虚假唤醒机制

模板：
```java
// synchronized - wait - notify
private volatile int num = 0;
public void addOne() throws Exception {
    // 0、加锁
    synchronized (this) {
        // 1、判断，不满足干活条件就阻塞
        while (num != 0) {
            this.wait();
        }
        // 2、干活
        num++;
        System.out.println(Thread.currentThread().getName() + " " + num);
        // 3、干完活唤醒其他线程
        this.notify();
    }
    // 0、解锁
}
// lock/unlock - await - signal
private volatile int num = 0;
private Lock lock = new ReentrantLock();
private Condition condition = lock.newCondition();
public void addOne() throws Exception {
    // 0、加锁
    lock.lock();
    try {
        // 1、判断，不满足干活条件就阻塞
        while (num != 0) {
            condition.await();
        }
        // 2、干活
        num++;
        System.out.println(Thread.currentThread().getName()+" "+num);
        // 3、干完活唤醒其他线程
        condition.signal();
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        // 0、解锁
        lock.unlock();
    }
}
```

## synchronized 与 Lock 的区别

1. 原始构成：
    synchronized 是关键字，属于 JVM 层面，使用字节码指令来控制锁：monitorenter和monitorexit。底层是通过 monitor 对象来完成的。（另外 wait 和 notify/notifyAll 也依赖于 monitor 对象，所以它们只能用于同步代码块中）；
    Lock 是一个接口，它有一堆的实现类，属于 API 层面。
2. 使用方法上：
    synchronized 是全自动的，自动获取锁、自动释放锁，无论正常退出还是发生异常都会释放锁
    Lock 需要自己来控制锁的获取和释放
3. 是否是公平锁：
    synchronized 是非公平锁，它无法保证等待的线程获取锁的顺序；
    ReentrantLook 可以选择是否公平锁。
4. 等待是否可以中断：
    synchronized 是不可中断锁；
    ReentrantLock 可以中断：1、可以通过 lockInterruptibly 方法中断等待锁；2、tryLock(long timeout, TimeUnit unit) 超时自动中断。
5. 是否支持精确唤醒：
    synchronized 不支持，要么随机唤醒一个（notify），要么全部唤醒（notifyAll）；
    ReentrantLock 通过 Condition 可以实现分组唤醒、精准唤醒。