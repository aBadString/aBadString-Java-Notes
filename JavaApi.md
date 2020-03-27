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

**【1、 HashMap的内部数据结构】**

jdk 1.8 ：内部使用数组 + 链表红黑树

![img](https://pic3.zhimg.com/80/v2-4ce38fd1c36fc61e70d7687fae997e5a_720w.jpg) 

当链表长度大于 8 时，转为红黑树；当红黑树节点小于 6 时，转为链表。

【2、】

