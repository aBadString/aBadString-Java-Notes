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

## 2.1 构造器 (4个)

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

## 2.2 HashMap的内部数据结构

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