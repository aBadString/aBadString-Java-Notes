<h1 id="Java虚拟机" align="center">Java 虚拟机</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. Java 内存区域](#1-java-内存区域)
  - [1.1. JVM 运行时数据区](#11-jvm-运行时数据区)
    - [1.1.1. 程序计数器 Program Counter Register](#111-程序计数器-program-counter-register)
    - [1.1.2. 虚拟机栈 Java Virtual Machine Stacks](#112-虚拟机栈-java-virtual-machine-stacks)
    - [1.1.3. 本地方法栈 Native Method Stacks](#113-本地方法栈-native-method-stacks)
    - [1.1.4. 堆 Heap](#114-堆-heap)
    - [1.1.5. 方法区 Method Area](#115-方法区-method-area)
    - [1.1.6. 运行时常量池](#116-运行时常量池)
    - [1.1.7. 直接内存](#117-直接内存)
  - [1.2. 对象访问](#12-对象访问)
- [2. 垃圾回收机制 GC](#2-垃圾回收机制-gc)
  - [2.1. 如何判断对象已死](#21-如何判断对象已死)
    - [2.1.1. 引用计数法 Reference Counting（Java 并不是）](#211-引用计数法-reference-countingjava-并不是)
    - [2.1.2. 根搜索算法 GC Roots Tracing](#212-根搜索算法-gc-roots-tracing)
  - [2.2. 引用](#22-引用)
    - [2.2.1. 强引用](#221-强引用)
    - [2.2.2. 软引用](#222-软引用)
    - [2.2.3. 弱引用](#223-弱引用)
    - [2.2.4. 虚引用](#224-虚引用)
  - [2.3. GC 是如何回收对象](#23-gc-是如何回收对象)
    - [2.3.1. finalize()方法](#231-finalize方法)
    - [2.3.2. 方法区垃圾回收](#232-方法区垃圾回收)
  - [2.4. 垃圾收集算法](#24-垃圾收集算法)
    - [2.4.1. 标记 — 清除算法](#241-标记-清除算法)
    - [2.4.2. 复制算法](#242-复制算法)
    - [2.4.3. 标记 — 整理算法](#243-标记-整理算法)
    - [2.4.4. 分代收集算法](#244-分代收集算法)
  - [2.5. 垃圾收集器](#25-垃圾收集器)
    - [2.5.1. Serial](#251-serial)
    - [2.5.2. ParNew](#252-parnew)
    - [2.5.3. Parallel Scavenge](#253-parallel-scavenge)
    - [2.5.4. Serial Old](#254-serial-old)
    - [2.5.5. Parallel Old](#255-parallel-old)
    - [2.5.6. CMS](#256-cms)
    - [2.5.7. G1](#257-g1)
  - [2.6. 内存分配策略](#26-内存分配策略)
  - [2.7. Java 中内存泄漏](#27-java-中内存泄漏)
  - [2.8. System.gc()](#28-systemgc)
  - [2.9. finalize()](#29-finalize)
- [3. 类文件结构](#3-类文件结构)
  - [3.1. Class 类文件结构](#31-class-类文件结构)
- [4. 类加载机制](#4-类加载机制)
  - [4.1. 类的生命周期](#41-类的生命周期)
  - [4.2. 触发类的初始化](#42-触发类的初始化)
  - [4.3. 类加载的过程](#43-类加载的过程)
  - [4.4. 类加载器](#44-类加载器)
    - [4.4.1. 双亲委派模型](#441-双亲委派模型)
- [5. 虚拟机字节码执行引擎](#5-虚拟机字节码执行引擎)
  - [5.1. 运行时栈帧结构](#51-运行时栈帧结构)
    - [5.1.1. 局部变量表](#511-局部变量表)
    - [5.1.2. 操作数栈](#512-操作数栈)
    - [5.1.3. 动态连接](#513-动态连接)
    - [5.1.4. 方法返回地址](#514-方法返回地址)
  - [5.2. 方法调用](#52-方法调用)
    - [5.2.1. 解析（类加载过程的解析阶段进行）](#521-解析类加载过程的解析阶段进行)
    - [5.2.2. 分派](#522-分派)
  - [5.3. 基于栈的指令集和基于寄存器的指令集](#53-基于栈的指令集和基于寄存器的指令集)
- [6. 高效并发](#6-高效并发)
  - [6.1. 指令重排序](#61-指令重排序)
  - [6.2. Java 内存模型与线程](#62-java-内存模型与线程)
    - [6.2.1. 主内存与工作内存](#621-主内存与工作内存)
    - [6.2.2. 内存间交互操作](#622-内存间交互操作)
    - [6.2.3. volatile](#623-volatile)
    - [6.2.4. 原子性、可见性和有序性](#624-原子性-可见性和有序性)
    - [6.2.5. 先行发生原则](#625-先行发生原则)
  - [6.3. Java 与线程](#63-java-与线程)
    - [6.3.1. 线程的实现](#631-线程的实现)
    - [6.3.2. Java 线程调度](#632-java-线程调度)
  - [6.4. 线程安全](#64-线程安全)
    - [6.4.1. 线程安全的实现方法](#641-线程安全的实现方法)
- [7. 锁优化](#7-锁优化)
- [8. JVM 调优](#8-jvm-调优)
  - [8.1. 三大参数（java 命令）](#81-三大参数java-命令)
    - [8.1.1. 标配参数](#811-标配参数)
    - [8.1.2. X 参数](#812-x-参数)
    - [8.1.3. XX 参数](#813-xx-参数)
    - [8.1.4. JVM 配置表](#814-jvm-配置表)
  - [8.2. JDK 工具](#82-jdk-工具)
    - [8.2.1. jps 虚拟机进程状况工具](#821-jps-虚拟机进程状况工具)
    - [8.2.2. jstat 虚拟机统计信息监视工具](#822-jstat-虚拟机统计信息监视工具)
    - [8.2.3. jinfo Java 配置信息工具](#823-jinfo-java-配置信息工具)
    - [8.2.4. jmap Java 内存映像工具](#824-jmap-java-内存映像工具)
    - [8.2.5. jhat 堆转储快照分析工具](#825-jhat-堆转储快照分析工具)
    - [8.2.6. jstack Java 堆栈跟踪工具](#826-jstack-java-堆栈跟踪工具)
    - [8.2.7. jconsole 可视化监视与管理控制台](#827-jconsole-可视化监视与管理控制台)

<!-- /code_chunk_output -->


JVM 包括：一套字节码指令集、一组寄存器、一个垃圾回收、一个栈、堆、一个存储方法域。JVM 是运行在操作系统之上的，它与硬件没有直接 的交互。 


# 1. Java 内存区域

## 1.1. JVM 运行时数据区

![image-20200319191553417](../images/image-20200319191553417.png)

### 1.1.1. 程序计数器 Program Counter Register
类似于计算机组成原理中的程序计数器 PC。它的作用可以看作是当前线程所执行的字节码的行号指示器。唯一一个不会导致内存泄漏的内存区域。
程序计数器是线程私有的，各个线程之间独立拥有一个程序计数器，互不影响。

### 1.1.2. 虚拟机栈 Java Virtual Machine Stacks
线程私有的
会产生```StackOverflowError```和```OutOfMemoryError```错误。
设置JVM参数 **-Xss228k**（栈大小为228k）

### 1.1.3. 本地方法栈 Native Method Stacks
会产生```StackOverflowError```和```OutOfMemoryError```错误。
程序计数器、虚拟机栈、本地方法栈随线程而生而灭。
本地方法栈是为 JVM 运行 Native 方法准备的空间，由于很多 Native 方法都是用 C 语言实现的，所以它通常又叫 **C 栈**。它与 Java 虚拟机栈实现的功能类似，只不过本地方法栈是描述本地方法运行过程的内存模型。

### 1.1.4. 堆 Heap
被所有线程所共享的，再虚拟机启动时被创建。
用于存放对象实例。
堆是垃圾收集器管理的主要区域。也被称为 GC 堆。
可以细分为：新生代和老年代。
**-Xms20M  -Xmx20M**（前者表示初始堆大小20M，后者表示最大堆大小20M）
会产生```OutOfMemoryError```错误。

### 1.1.5. 方法区 Method Area
被所有线程所共享的。
用于存储：已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等。
别名：Non-Heap
会产生```OutOfMemoryError```错误。

### 1.1.6. 运行时常量池
属于方法区的一部分，用于存放：编译期生成的各种字面量和符号引用
会产生```OutOfMemoryError```错误。

### 1.1.7. 直接内存
直接内存是除 Java 虚拟机之外的内存，但也可能被 Java 使用。

## 1.2. 对象访问
1. 句柄  
![image-20200319194643432](../images/image-20200319194643432.png)
2. 直接指针  
![image-20200319194702842](../images/image-20200319194702842.png)


# 2. 垃圾回收机制 GC

## 2.1. 如何判断对象已死

### 2.1.1. 引用计数法 Reference Counting（Java 并不是）

微软的 COM 是

算法：给对象添加一个引用计数器，每次被引用时，计数器自增；当引用失效时，计数器自减。计数器的值为0的对象不能被再次使用。

无法解决：对象之间相互循环引用的问题。

### 2.1.2. 根搜索算法 GC Roots Tracing

Java、C#、Lisp 是

算法：通过一系列名为 "GC Roots" 的对象作为起点，开始向下搜索。当从 GC Roots 到一个对象不可达时，则该对象是不可用的。
这就解决了对象之间形成闭环的问题。

GC Roots 包括：
1. 虚拟机栈中的引用的对象
2. 本地方法栈中 JNI 的引用的对象
3. 方法区中类静态属性引用的对象
4. 方法区中常量引用的对象

**【我的思考】：那么对于单例对象呢？自身形成闭环怎么处理的。**
答：单例对象由自身类的静态属性所引用。

## 2.2. 引用

### 2.2.1. 强引用
像 ```Integer i = new Integer()```这种的。
GC 永远也不会回收强引用的对象。

### 2.2.2. 软引用
再系统将要内存溢出时，不会马上回收软引用的对象，只会把软引用的对象列入第二次回收的清单当中。如果第一次回收结束后还是没有足够内存，会进行第二次回收，回收软引用对象。
JDK 1.2 之后，使用 ```SoftReference``` 类来实现

### 2.2.3. 弱引用
弱引用的对象会在接下来的一次垃圾回收的时候被清理掉。
JDK 1.2 之后，使用 ```WeakReference``` 类来实现

### 2.2.4. 虚引用
**虚引用和上述三者不是并列关系**，前三者和对象的生命周期密切相关。而虚引用的目的是为了在这个对象被回收时得到一个系统消息，不会改变对象的生命周期。
JDK 1.2 之后，使用 ```PhantomReference``` 类来实现

虚引用必须和引用队列 （ReferenceQueue）联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之 关联的引用队列中。

## 2.3. GC 是如何回收对象

### 2.3.1. finalize()方法
当一个对象没有重写```finalize()```方法，或者```finalize()```方法已经被执行过一次 都不会执行```finalize()```方法。

任何一个对象的```finalize()```方法都只会被系统自动调用一次，在下一次垃圾回收时```finalize()```方法不会被再次执行。

JVM只保证`finalize()`方法会被调用，但不保证方法里的任务会被执行完

### 2.3.2. 方法区垃圾回收

主要回收：废弃常量 和 无用的类

判断是否为废弃常量：

1. 该类所有的实例都已不存在
2. 加载该类的 ClassLoader 已经被回收
3. 该类对于的 Class 对象没有在任何地方被引用，无法通过反射来访问该类

## 2.4. 垃圾收集算法

### 2.4.1. 标记 — 清除算法

算法：先标记出需要回收的对象，然后统一回收所有被标记的对象。

缺点：
1. 标记和清除的效率都不高
2. 清除之后会产生大量不连续的内存碎片

### 2.4.2. 复制算法

算法：将可用内存分为等量的两块，每次只在其中一块内存上分配对象的空间，另一块内存全部未使用。当第一块内存用完之后，把还需要的对象依次拷贝到第二块内存中，然后把第一块内存全部回收。

解决了：内存碎片的问题

优点：实现简单、运行高效

缺点：将可用内存缩小为了原来的一半

用来回收新生代，将内存分为一块较大的和两块较小的。每次使用一块大的和一块小的，回收时，将需要的对象拷贝到另一块小的上面，清理另外两块。比例默认是 8 : 1 : 1。

### 2.4.3. 标记 — 整理算法

算法：先标记出要被回收的对象，然后将所有存活的对象都向堆的一边移动，清理掉边界之外的内存。

**很像数组中删除中间某个元素：后一个元素不断往前拷贝，最后一个元素置为 null。**

### 2.4.4. 分代收集算法

算法：将内存分为新生代和老年代。新生代中的对象存活期短，每次标记时会有大量对象死去，采用复制算法清理；老年代对象存活较久，可以使用 标记 — 清除算法 或 标记 — 整理 算法。

## 2.5. 垃圾收集器

![image-20200319212916442](../images/image-20200319212916442.png)

连线表示两个收集器可以搭配使用。

### 2.5.1. Serial

新生代、单线程 的收集器。在它工作时必须暂停其他线程（Stop The World）。

优点：简单高效

适合：Client 模式下新生代收集器。

![image-20200319213437238](../images/image-20200319213437238.png)

### 2.5.2. ParNew

Serial 的多线程版本。也会有 Stop The World。

适合：Server 模式下新生代收集器。

![image-20200319213446924](../images/image-20200319213446924.png)

### 2.5.3. Parallel Scavenge

使用复制算法、并行的 多线程、新生代 收集器。

目标：达到一个可控制的吞吐量，吞吐量 = 运行用户代码时间 / (运行用户代码时间 + 垃圾收集时间)。即就是，尽可能缩短垃圾回收的时间。

![image-20200319215013138](../images/image-20200319215013138.png)

### 2.5.4. Serial Old

Serial 的老年代版本，单线程，使用 标记 — 整理 法。

适合：Client 模式下老年代收集器，jdk 1.5 以及之前 Server 模式和 Parallel Scavenge 搭配，作为 CMS 的后备。

![image-20200319213437238](../images/image-20200319213437238.png)

### 2.5.5. Parallel Old

Parallel Scavenge 的老年代版本，多线程，使用 标记 — 整理 法。

![image-20200319215013138](../images/image-20200319215013138.png)

### 2.5.6. CMS

以获得最短回收停顿时间为目标，基于 标记 — 清除 算法。

四个步骤：
1. 初始标记：标记一下 GC Roots 能够直接关联到的对象（速度很快）
2. 并发标记：执行 GC Roots Tracing
3. 重新标记：修正并发标记期间，因用户程序运行而产生的变动（比初始标记稍长，但远短于并发标记的时间）
4. 并发清除

初始标记 和 重新标记 仍然会 Stop The World。

优点：并发收集、低停顿

缺点：
- CMS 对 CPU 资源非常敏感
- 无法处理浮动垃圾
- 使用了 标记 — 清除 算法，会产生大量内存碎片

![image-20200319215720204](../images/image-20200319215720204.png)

### 2.5.7. G1

相对CMS的改进：
- 基于 标记 — 整理 算法
- 它可以非常准确地控制停顿

## 2.6. 内存分配策略

1. **对象优先分配在新生代**
2. **需要大量连续内存的对象分配到老年代**
3. **长期存活的对象将进入老年代**
当一个对象在新生代每经过一次 GC，年龄加一，当到15岁时进入老年代。
4. **动态年龄判定**
当新生代中相同年龄的对象大小之和大于内存空间一半时，年龄大于或等于该年龄的对象直接进入老年代。
5. **空间分配担保**
![image-20200319221452552](../images/image-20200319221452552.png)


## 2.7. Java 中内存泄漏

所谓内存泄露就是指一个不再被程序使用的对象或变量一直被占据在内存中。

Java 中有垃圾回收机制，它可以保证一对象不再被引用的时候，即对象变成了孤儿的时候，对象将自动被垃圾回收器从内存中清除掉。

Java 中的内存泄露的情况：长生命周期的对象持有短生命周期对象的引用就很可能发生内存泄露，尽管短生命周期对象已经不再需要，但是因为长生命周期对象持有它的引用而导致不能被回收，这就是 Java 中内存泄露的发生场景，通俗地说，就是程序员可能创建了一个对象，以后一直不再使用这个对象，这个对象却一直被引用，即这个对象无用但是却无法被垃圾回收器回收的，这就是 java 中可能出现内存泄露的情况，例如，缓存系统，我们加载了一个对象放在缓存中 (例如放在一个全局 map 对象中)，然后一直不再使用它，这个对象一直被缓存引用，但却不再被使用。

## 2.8. System.gc()

这个方法用来提示 JVM 要进行垃圾回收。但是，立即开始还是
延迟进行垃圾回收是取决于 JVM 的。
```java
package java.lang;
public final class System {
  public static void gc() {
      Runtime.getRuntime().gc();
  }
}
```
```java
package java.lang;
public class Runtime {
    private static Runtime currentRuntime = new Runtime();

    public static Runtime getRuntime() {
        return currentRuntime;
    }
    public native void gc();
}
```

## 2.9. finalize()
垃圾回收器（garbage colector）决定回收某对象时，就会运行该对象的 finalize() 方法
它最主要的用途是回收特殊渠道申请的内存。Java 程序有垃圾回收器，所以一般情况下内存问题不用程序员操心。
但有一种 JNI（Java Native Interface）调用 non-Java 程序（C 或 C++），finalize() 的工作就是回收这部分的内存。


# 3. 类文件结构

## 3.1. Class 类文件结构

Java 虚拟机具有语言无关性。只要是符合 Java 虚拟机规范的字节码文件，就可以被虚拟机执行。
Java 虚拟机具有平台无关性。不同操作系统上都有相应的虚拟机版本。

1. 魔数：Class 文件前4字节，用来标识 Class 文件。值为，0xCAFEBASE。
2. 版本号：接下来的4字节，第5、6字节是次版本号，第7、8字节是主版本号。
3. 常量池入口：紧后的2字节

常量池中存放：字面量和符号引用；符号引用包括：类和接口的全限定名、字段的名称和描述符、方法的名称和描述符。


# 4. 类加载机制

## 4.1. 类的生命周期

类的整个生命周期 (7阶段)：加载 (Loading)、验证 (Verification)、准备 (Preparation)、解析 (Resolution)、初始化 (Initialization)、使用 (Using)、卸载 (Unloading)。

![image-20200321162436004](../images/image-20200321162436004.png) 

## 4.2. 触发类的初始化

![image-20200321162806300](../images/image-20200321162806300.png)

## 4.3. 类加载的过程

(1) 加载阶段，虚拟机要完成以下3件事情：
1. 通过类的全限定名，获取其二进制字节流
2. 将该字节流的静态存储结构转化为方法区的运行时数据结构
3. 在堆中生成一个代表该类的 java.lang.Class 对象，作为方法区的数据的访问入口

(2) 验证阶段，四个验证
1. 文件格式验证
2. 元数据验证
3. 字节码验证
4. 符号引用验证

(3) 准备阶段：为static的类变量分配内存并设置默认初始值（0、\u0000、null）
public static int value=123; // 在准备阶段 value 初始值为 0 。在初始化阶段才会变为 123

(4) 解析阶段：将常量池中的符号引用替换为直接引用的过程
![image-20200321163809920](../images/image-20200321163809920.png)
1. 类或接口的解析
2. 字段解析
3. 类方法解析
4. 接口方法解析

(5) 初始化阶段：类加载的最后一步
前面的类加载过程，除了在加载阶段用户应用程序可以通过自定义类加载器参与之外，其余动作完全由虚拟机主导和控制。
到了初始化阶段，才真正开始执行类中定义的Java 程序代码。
![image-20200321164156083](../images/image-20200321164156083.png)


## 4.4. 类加载器

### 4.4.1. 双亲委派模型

Java虚拟机角度分：两种不同的类加载器：
1. 启动类加载器 (Bootstrap ClassLoader)，由C++实现，是虚拟机自身的一部分
2. 所有其他的类加载器，由Java实现，独立于虚拟机之外，全部继承自java.lang.ClassLoader

程序员角度分：三种系统提供的类加载器，一种自定义的：
1. 启动类加载器 (Bootstrap ClassLoader)
2. 拓展类加载器 (Extension ClassLoader)
3. 应用程序类加载器 (Application ClassLoader)
4. 自定义类加载器 (User ClassLoader)

![image-20200321165440524](../images/image-20200321165440524.png)

双亲委派模型要求除了启动类加载器外，其余的都需要有父类加载器。但不会使用继承，而是使用组合关系来实现。

双亲委派的工作过程：当一个类收到了类加载请求时，会先委派给父类加载器去完成，每层都会传递加载请求。只有当父类加载器无法完成时，子类加载器才会尝试去做。

- 隐式加载指的是程序在使用 new 等方式创建对象时，会隐式地调用类的加载器把对应的类加载到 JVM 中。
- 显示加载指的是通过直接调用 Class.forName() 方法来把所需的类加载到 JVM 中。
- 未找到指定类文件会抛出 ClassNotFountException

# 5. 虚拟机字节码执行引擎

1. 解释执行：通过解释器执行
2. 编译执行：通过即时编译器产生本地代码执行

## 5.1. 运行时栈帧结构

栈帧是虚拟机运行时数据区中的虚拟机栈的栈元素。
栈帧中存储了：方法的局部变量表、操作数栈、动态链接、方法返回地址等信息。
当前栈帧：栈顶的栈帧。所关联的方法称为当前方法。

### 5.1.1. 局部变量表

局部变量表的容量以变量槽（Variable Slot）为最小单位。用于存储方法的入参和方法内的局部变量。
在代码被编译为 class 文件时，就在方法的 Code 属性的 max_locals 中确定了局部变量表的最大容量。
每个 Slot 的大小并不是固定的，和具体的虚拟机位数有关，但规定了一个 Slot 可以存放下 boolean byte char short int float reference returnAddress，long double 用两个 Slot 存储。
非静态方法的 0 号 Slot 是方法所属对象的引用，接着按参数表顺序放入实参，然后是局部变量。
Slot 是可重用的，当一个局部变量离开它的作用域后，它会被出栈。（但不会马上删掉这个 Slot 上的数据，会在下一个局部变量入栈时覆盖，也就是说在此之前，旧的变量还是存在的，不能被 GC。看下面例子：）

在虚拟机参数加上 `-verbose:gc` 以便看GC的过程
```java
public static void main(String[] args) {
    byte[] holder = new byte[64 * 1024 * 1024];
    System.gc();
}
// [GC (System.gc())  69468K->66288K(251392K), 0.0011545 secs]
// [Full GC (System.gc())  66288K->66152K(251392K), 0.0047648 secs]
// 还有 66152K 内存没有被回收，这是正常的，因为holder还没有离开作用域

// 改变一下holder的作用域
// 这下总该回收了吧
public static void main(String[] args) {
    {
        byte[] holder = new byte[64 * 1024 * 1024];
    }
    System.gc();
}
// [GC (System.gc())  69468K->66312K(251392K), 0.0008585 secs]
// [Full GC (System.gc())  66312K->66152K(251392K), 0.0044798 secs]
// 事实是仍然没有
// 原因是虽然 holder 出栈了，但是它的数据还没有被抹去

// 我们将一个局部变量入栈，覆盖 holder 的数据看看
public static void main(String[] args) {
    {
        byte[] holder = new byte[64 * 1024 * 1024];
    }
    System.gc();
    int a = 1;
    System.gc();
}
// [GC (System.gc())  69468K->66296K(251392K), 0.0009442 secs]
// [Full GC (System.gc())  66296K->66152K(251392K), 0.0045894 secs]
// [GC (System.gc())  66152K->66152K(251392K), 0.0005715 secs]
// [Full GC (System.gc())  66152K->616K(251392K), 0.0033609 secs]
// 可以看到第二次 GC 把这 64M 空间回收了

// 还有一种方法，将 holder 置空也能帮助 GC
public static void main(String[] args) {
    {
        byte[] holder = new byte[64 * 1024 * 1024];
        holder = null; // help GC
    }
    System.gc();
}
// [GC (System.gc())  69468K->66312K(251392K), 0.0009519 secs]
// [Full GC (System.gc())  66312K->616K(251392K), 0.0044553 secs]
// 这一方法在 java.util.concurrent.locks.AbstractQueuedSynchronizer 的 final boolean acquireQueued(final Node node, int arg) 中也有
```

局部变量与类变量的初始化不同：
类变量有两次赋值操作：准备阶段赋默认值，初始化阶段显示赋值；局部变量没有赋默认值这一阶段，只有显示赋值。

### 5.1.2. 操作数栈

在代码被编译为 class 文件时，就在方法的 Code 属性的 max_stacks 中确定了操作数栈的最大深度。

### 5.1.3. 动态连接

### 5.1.4. 方法返回地址

退出方法的两种方式：
1. 正常完成出口：遇到方法返回的字节码指令，根据方法的声明来给出返回值
2. 异常完成出口：遇到异常，且异常没有在方法中处理，不会有任何返回值

## 5.2. 方法调用

方法调用并不等于方法执行。方法调用阶段的唯一任务是：确定被调用方法的版本，即调用哪一个方法。
class 文件的编译是没有**链接**这个过程的。所有的方法调用在 class 文件中都存储的是**符号引用**，而不是**直接引用**（运行时内存中的方法入口地址）。
而符号引用转化为直接引用的过程是在：类加载阶段 或 运行期间进行的。

### 5.2.1. 解析（类加载过程的解析阶段进行）

解析：如果当方法的调用版本在运行期间是不可改变的，那么在类加载阶段，会把一部分符号引用转化为直接引用。
编译器可知，运行期不可变 — 静态方法和私有方法。这两种方法都不能被重写。静态方法直接与类型关联，私有方法在外部不可访问。

JVM 中有四条方法调用的字节码指令：
invokestatic：调用静态方法
invokespecial：调用实例构造器`<init>`方法、私有方法、父类方法。
invokevirtual：调用所有虚方法（特别的 final 所修饰的方法也是使用 invokevirtual 来调用，但是它是非虚方法，不能被重写）
invokeinterface：调用接口方法（抽象方法），运行时才确定实现类的对象的方法

被 invokestatic 和 invokespecial 调用的方法解析阶段可以确定版本（静态方法、私有方法、构造器、父类方法），在类加载时将符号引用解析为直接引用。
以上四类方法 加上 final 修饰的方法，称为非虚方法。

### 5.2.2. 分派

- **静态分派**：所有依赖静态类型来定位方法执行版本的分派动作，方法重载。静态分派发生在编译期间。

```java
public class OverLoad {
    public void say(Human h) {
        System.out.println("Human");
    }
    public void say(Man h) {
        System.out.println("Man");
    }
    public void say(Woman h) {
        System.out.println("Woman");
    }

    public static void main(String[] args) {
        OverLoad load = new OverLoad();
        Human h1 = new Man();
        Human h2 = new Woman();
        load.say(h1);
        load.say(h2);
    }
}
// Human
// Human
```
编译器在决定使用那个重载方法时是通过参数的**静态类型**来做判断的。

```java
public class MethodTest {
    public static void main(String[] args) {
        int a = 0;
        method(a);
    }

    // 以下五个方法按照调用选择顺序排列
   //方法1
    public static void method(int a){
        System.out.println("执行method(int a)");
    }
    //方法2
    public static void method(long a){
        System.out.println("执行method(long a)");
    }
    //方法3
    public static void method(Integer a) {
        System.out.println("执行method(Integer a)");
    }
    //方法4
    public static void method(Object a) {
        System.out.println("执行method(Object a)");
    }
    //方法5
    public static void method(int...a) {
        System.out.println("执行method(int...a)");
    }
}


class A {
    public static void method(int a){
        System.out.println("A类中的method()");
    }
}
public class MethodTest2 extends A{
    //方法4
    public static void method(Object a) {
        System.out.println("执行method(Object a)");
    }
    //方法5
    public static void method(int...a) {
        System.out.println("执行method(int...a)");
    }
    public static void main(String[] args) {
        int a = 0;
        method(a);
    }
}
```
关于方法重载时的调用选择我们可以得出以下结论：
(1)   精确匹配：对于上述代码中，当有method(int a)存在时调用的肯定就是这个方法；
(2)   自动类型提升：对于基础数据类型，自动转成表示范围更大的类型；当方法1被注释的时候，会去调用method(long a)而不是method(Integer a)；
(3)   自动装箱与拆箱：当方法1，2被注释，就调用方法3；
(4)   根据子类依次向上继承路线匹配：当只有方法4与方法5时，先找int的父类，找到的object类型，匹配之后调用；当继承A类之后，由于本类没有合适的方法，然后就去A类中找，匹配调用（A类中方法参数类型换成long，Integer结果也一样）；
(5)   根据可变参数匹配。
引用自：https://www.jianshu.com/p/306c4bfe3f54


- **动态分派**：方法重写
通过方法的接收者来决定使用那个方法。运行期间通过实际类型来确定方法的调用版本。
Java 中其实没有虚函数的概念，它的普通函数就相当于 C++ 的虚函数，**动态绑定**是Java的默认行为。如果 Java 中不希望某个函数具有虚函数特性，可以加上 final 关键字变成非虚函数。

- **单分派与多分派**：

方法的宗量：方法的接收者与方法的参数统称为方法的宗量。
单分派：根据一个宗量选择目标方法；
多分派：根据多个宗量选择目标方法。
重载时：根据不同方法接收者的静态类型和不同参数类型的方法中选择，即在根据两个个宗量来选择方法；
重写时：重载已经在解析阶段确定了，运行时只需要根据方法的接收者的实际类型来选择方法，即根据一个宗量来选择方法。
所以，Java 语言：静态多分派，动态单分派。
C# 3.0 以前和 Java 一样，4.0 引入了 dynamic 类型后，实现了动态多分派。

例子：
```java
class A{}
class B{}
class Super {
    public void say(A a) {
        System.out.println("Super A");
    }
    public void say(B b) {
        System.out.println("Super B");
    }
}
class Son extends Super{
    @Override
    public void say(A a) {
        System.out.println("Son A");
    }
    @Override
    public void say(B b) {
        System.out.println("Son B");
    }
}

public class ZhongLiang {
    public static void main(String[] args) {
        Super sup = new Super();
        Super son = new Son();

        sup.say(new A());
        son.say(new B());
    }
}
// Super A
// Son B
```
上述有四个方法：
1、Super::say(A)
2、Super::say(B)
3、Son::say(A)
4、Son::say(B)

12 重载，34重载；13重写，24重写；

静态分派需要根据 接收者的静态类型是Super或者Son 以及 方法参数类型是A或者B 这两个宗量来选择；结果是选择了 Super::say(A) 和 Super::say(B)
动态分派：由于静态分配期间已经确定了方法签名这一个宗量，所以只需要根据 接收者的实际类型是Super或者Son 这一个宗量来选择；结果是选择了 Super::say(A) 和 Son::say(B)

## 5.3. 基于栈的指令集和基于寄存器的指令集

Java 编译器输出的指令流基本上是一种基于栈的指令集架构（栈是指操作数栈）。
基于栈：
​	优点：可移植性，代码相对更紧凑，编译器实现更简单
​	缺点：执行速度相对稍慢

安卓的 Dalvik 虚拟机是基于寄存器的架构。

![image-20200321185141401](../images/image-20200321185141401.png)

# 6. 高效并发

## 6.1. 指令重排序

![image-20200324202935391](../images/image-20200324202935391.png)

## 6.2. Java 内存模型与线程

### 6.2.1. 主内存与工作内存

以下涉及到的**变量**均专指：实例字段、静态字段、构成数组对象的元素，是可以被多个线程共享的。

局部变量和方法参数是线程私有的，不会被共享。

所有的变量都存储在主内存中，此外，每条线程都有自己的工作内存。在工作内存中保存了被该线程使用到的变量的主内存的副本。线程对变量的操作只能在工作内存中进行，不能直接操作主内存中的变量。

**哦 ~ ，那 volatile 变量 和 直接内存！**

![image-20200324204556331](../images/image-20200324204556331.png)

### 6.2.2. 内存间交互操作

作用于主内存的变量：
1. lock 锁定：把一个变量标识位一条线程独占的状态
2. unlock 解锁：释放处于锁定状态的变量
3. read 读取：把一个变量从主内存传输到线程的工作内存中，以便随后的 load 操作使用
4. write 写入：把 store 操作从工作内存中得到的变量的值，放入主内存的变量中

作用于工作内存的变量：
5. load 载入：把 read 操作从主内存中得到的变量的值，放入工作内存的变量副本中
6. use 使用：把工作内存中一个变量的值传递给执行引擎（每当遇到一个需要使用变量的值的字节码指令时，虚拟机会执行这个操作）
7. assign 赋值：把一个从执行引擎接受到的值赋值给工作内存的变量（每当遇到一个给变量赋值的字节码指令时，虚拟机会执行这个操作）
8. store 存储：把工作内存中的一个变量的值传输到主内存中，以便随后的 write 操作使用

![image-20200324205933996](../images/image-20200324205933996.png)
![image-20200324210114109](../images/image-20200324210114109.png)

### 6.2.3. volatile

volatile 变量的特性：

1. 保证该变量对所有线程可见
2. 禁止指令重排序优化

【《深入理解Java虚拟机》332 页 代码清单 12-7】
![image-20200324215527107](../images/image-20200324215527107.png)

### 6.2.4. 原子性、可见性和有序性

**原子性**：Java 内存模型中来直接保证原子性的变量操作包括 read、load、assign、use、store、write。我们认为基本数据类型的访问读写是具备原子性的。在 synchronized 块之间的操作也具备原子性。

*非原子的 64 位操作：没有被声明位 volatile 的 64 位数值变量 (double、long)，JVM 允许将64位的读写划分为两个 32 位操作。*

**可见性**：是指当一个线程修改了共享变量的值，其他线程能够立即得知。

**有序性**：如果在本线程内观察，所有的操作都是有序的；如果在一个线程中观察另一个线程，所有的操作都是无序的。前半句指："线程内表现位串行的语义"，后半句指："指令重排序"现象和"工作内存与主内存同步延迟"现象。

### 6.2.5. 先行发生原则

![image-20200324220021374](../images/image-20200324220021374.png)
![image-20200324220241828](../images/image-20200324220241828.png)

时间上的先后顺序与先行发生原则之间基本没有太大关系，所以**我们衡量并发安全问题的时候**不要受到时间顺序的干扰，**一切必须以先行发生原则为准**。

并发并不一定要依赖多线程，如 PHP 中常见的**多进程**并发。

## 6.3. Java 与线程

### 6.3.1. 线程的实现

**1. 使用内核线程实现**

内核线程就是直接由操作系统内核支持的线程，这种线程由内核来进行线程调度。

**2. 使用用户线程实现**

![image-20200324222401487](../images/image-20200324222401487.png)

**3. 使用用户线程加轻量级进程混合实现**

### 6.3.2. Java 线程调度

**6 种状态**：

1. 新建 New：创建后尚未启动
2. 运行 Runable：包括操作系统中的 Running 和 Ready，即执行态和就绪态
3. 无期限等待 Waiting：这种状态的线程不会被分配CPU，要等待被其他线程显式地唤醒
   三种方法进入这个状态：
   1、没有设置 Timeout 的 Object.wait() 方法  
   2、没有设置 Timeout 的 Thread.join() 方法
   3、LockSupport.park() 方法
4. 期限等待 Timed Waiting：这种状态的线程不会被分配CPU，在一定时间后被系统唤醒
   五种方法进入这个状态：
   1、设置 Timeout 了的 Object.wait() 方法  
   2、设置 Timeout 了的 Thread.join() 方法
   3、Thread.sleep() 方法
   4、LockSupport.parkNanos() 方法
   5、LockSupport.parkUntil() 方法
5. 阻塞 Blocked：等待一个排它锁
6. 结束 Terminated：线程已经执行结束

![image-20200326203103878](../images/image-20200326203103878.png)

## 6.4. 线程安全

按照线程安全的安全程度分：
1. 不可变：final、java.lang.String的对象、枚举、Integer是不可变对象。
2. 绝对线程安全：不论何种环境下，不需要做任何额外同步措施。
3. 相对线程安全：对这个对象单独操作时是线程安全的，不需要额外的同步措施；但是对于某些特定顺序的连续调用，就可能需要使用额外的同步手段保证调用的正确性。Vector、HashTable 等同步容器。（这一级别就是我们通常讲的线程安全）
4. 线程兼容：对象本身不是线程安全的，可以通过调用段使用同步手段来保证对象安全。HashMap、ArrayList。（这一级别就是我们通常讲的线程不安全）
5. 线程对立：不管采用哪种方式，都不能并发的使用。（suspend和resume；System.setIn和System.setOut和System.runFinalizersOnExit）

### 6.4.1. 线程安全的实现方法

**1、互斥同步**

同步：在多个线程并发访问共享数据时，保证共享数据在同一时刻只被一条线程使用。
互斥：是实现同步的方法。有 临界区、互斥量、信号量。

【synchronized 重量级锁】

![image-20200326203653161](../images/image-20200326203653161.png)

1. synchronized 是可以重进入的。
2. 当有其他线程访问同步块会被阻塞。阻塞或者唤醒一条线程涉及到用户态转换为核心态。

【ReentrantLock 重入锁】

比 synchronized 增加了一些高级功能

1. 等待可中断：当锁长期不被释放时，等待的线程可以选择放弃等待

2. 可实现公平锁：

   公平锁：必须按照申请锁的先后顺序来活得锁

   非公平锁：在锁被释放时，任何一个等待线程都有机会获得

   synchronized 是非公平，ReentrantLock 默认非公平，可由构造函数指定使用公平锁

3. 锁可以绑定多个条件：一个 ReentrantLock 对象可以同时绑定多个 Condition 对象

![image-20200326204817398](../images/image-20200326204817398.png)

**2、非阻塞同步**

互斥同步也叫做阻塞同步，它在阻塞和唤醒线程时会带来性能问题。

操作和冲突检测具备原子性，由硬件支持。

![image-20200326205209303](../images/image-20200326205209303.png)

【比较并交换 CAS】jdk1.5 之后

这个指令需要三个操作数：内存位置 V，旧的预期值 A，新值 B。

当且仅当 V 的值 符合 A 时，CPU 用 B 更新 V 的值，否则不更新；但都会返回 V 的旧值。

J.U.C 包中的整数原子类即使用了 CAS 操作

![image-20200326210140921](../images/image-20200326210140921.png) 

**3、无同步方案**

可重入代码一定线程安全。

线程本地存储

# 7. 锁优化

【自旋锁】
![image-20200326213802024](../images/image-20200326213802024.png)
jdk1.4.2时引入了，使用 -XX:+UseSpinning 开启；jdk1.6默认开启。默认次数时10次，可以使用 -XX:PreBlockSpin 修改。

自适应的自旋锁
![image-20200326214052289](../images/image-20200326214052289.png)


【锁消除】
虚拟机即时编译器在运行时，对一些代码上要求同步的，但被检测到没有共享数据竞争的锁进行消除，即编译器优化。
锁消除的主要判断依据来源于逃逸分析的数据支持。

【锁粗化】
将相邻的一些可以被合并的锁给合并。

【偏向锁】—【轻量级锁】—【重量级锁】
偏向锁使用 -XX:UseBiasedLocking 启用，jdk1.6默认开启

Mark Word
![](../images/JVM/MarkWord.png)

![image-20200413162304323](../images/image-20200413162304323.png)

 
# 8. JVM 调优

## 8.1. 三大参数（java 命令）

### 8.1.1. 标配参数

- java -version，获取JDK版本
- java -help，获取帮助
- java -showversion，获取JDK版本和帮助

```shell
java -version

openjdk version "1.8.0_265"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_265-b01)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.265-b01, mixed mode)
```

### 8.1.2. X 参数

X 参数用于指定字节码的执行模式（java -version 命令输出的最后一行 mixed mode 表示混合模式）。

- java -Xint：直接解释执行  interpreted mode
- java -Xcomp：先编译成本地代码再执行  compiled mode
- java -XMixed：混合模式（既有编译执行也有解释执行）  mixed mode

### 8.1.3. XX 参数

XX 参数有两种类型，一种是 Boolean 类型，另外一种是键值对类型。

**Boolean 类型**
`-XX:+属性名` 或者 `-XX:-属性名`
+表示开启了这个属性，-表示关闭了这个属性。
例如：
-XX:-PrintGCDetails：表示关闭GC详情输出
-XX:+PrintGCDetails：表示开启GC详情输出

```shell
java -XX:+PrintGCDetails HelloJVM
```

**key-value类型**
`-XX:key=value`
例如：
-XX:metaspace=2000000：设置Java元空间的值为2000000。
-XX:MetaspaceSize=128m：设置Java元空间的值为128M。
-Xms == -XX:InitialHeapSize ，初始化堆内存（默认只会用最大物理内存的1/64）
-Xmx == -XX:MaxHeapSize ，最大堆内存（默认只会用最大物理内存的1/4）
-Xss == -XX:ThreadStackSize ，设置单个线程的栈大小，一般默认为512K~1024K
-Xmn ，堆内新生代的大小。通过这个值也可以得到老生代的大小：-Xmx减去-Xmn

```shell
java -XX:InitialHeapSize=200m HelloJVM
# 或者
java -Xms200m HelloJVM


java -XX:MaxHeapSize=200M HelloJVM
# 或者
java -Xmx200m HelloJVM
```

**查看XX配置的命令**

使用 `jinfo -flag` 指令可以查看各个java进程的参数设置情况
```shell
# 查看元空间大小
# MetaspaceSize: 属性名
# 12208: Java 进程ID，使用 jps指令查看（jps -l 可以列出 id 和 类名）
jinfo -flag MetaspaceSize 15048
-XX:MetaspaceSize=21807104
# 可以看到元空间大小是 21 M

# 查看GC详情输出是否开启
jinfo -flag PrintGCDetails 15852
-XX:+PrintGCDetails
```

```shell
# 查看出厂默认设置的所有XX配置项
java -XX:+PrintFlagsInitial -version

# 查看 JVM 当前所有XX配置项
java -XX:+PrintFlagsFinal -version

# 也可以在运行程序时打印配置
java -XX:+PrintFlagsFinal HelloJVM

# 查看 JVM 自动配置的或者用户手动设置的XX选项（非应用程序的）
java -XX:+PrintCommandLineFlags -version
```

### 8.1.4. JVM 配置表

1. 堆设置
-Xms:初始堆大小
-Xmx:最大堆大小
-Xmn:新生代大小
-XX:NewRatio:设置新生代和老年代的比值。如：为3，表示年轻代与老年代比值为1：3
-XX:SurvivorRatio:新生代中Eden区与两个Survivor区的比值。注意Survivor区有两个。如：为3，表示Eden：Survivor=3：2，一个Survivor区占整个新生代的1/5  
-XX:MaxTenuringThreshold:设置转入老年代的存活次数。如果是0，则直接跳过新生代进入老年代
-XX:PermSize、-XX:MaxPermSize:分别设置永久代最小大小与最大大小（Java8以前）
-XX:MetaspaceSize、-XX:MaxMetaspaceSize:分别设置元空间最小大小与最大大小（Java8以后）

2. 收集器设置
-XX:+UseSerialGC:设置串行收集器
-XX:+UseParallelGC:设置并行收集器
-XX:+UseParalledlOldGC:设置并行老年代收集器
-XX:+UseConcMarkSweepGC:设置并发收集器

3. 垃圾回收统计信息
-XX:+PrintGC
-XX:+PrintGCDetails
-XX:+PrintGCTimeStamps
-Xloggc:filename

4. 并行收集器设置
-XX:ParallelGCThreads=n:设置并行收集器收集时使用的CPU数。并行收集线程数。
-XX:MaxGCPauseMillis=n:设置并行收集最大暂停时间
-XX:GCTimeRatio=n:设置垃圾回收时间占程序运行时间的百分比。公式为1/(1+n)

5. 并发收集器设置
-XX:+CMSIncrementalMode:设置为增量模式。适用于单CPU情况。
-XX:ParallelGCThreads=n:设置并发收集器新生代收集方式为并行收集时，使用的CPU数。并行收集线程数。



## 8.2. JDK 工具

### 8.2.1. jps 虚拟机进程状况工具

列出正在运行的虚拟机进程，显示：执行主类的类名（main方法所在类）、进程的本地虚拟机唯一ID（LVMID）

|参数|含义|
|:-:|:-:|
|-q|只输出LVMID|
|-l|输出主类的全类名；如果执行的是jar包，输出jar路径|
|-m|输出main方法的命令行参数|
|-v|输出JVM参数|

```shell
java -Xmx200m jvm.XX hello args

PS D:\MyProject\IdeaProjects\java> jps
11904 XX
5684 Jps
10828 Launcher

PS D:\MyProject\IdeaProjects\java> jps -q
11904
3312
10828

PS D:\MyProject\IdeaProjects\java> jps -l
1040 sun.tools.jps.Jps
11904 jvm.XX
10828 org.jetbrains.jps.cmdline.Launcher

PS D:\MyProject\IdeaProjects\java> jps -m
11904 XX hello args
10828 Launcher C:/Program Files/DevTools/IntelliJ IDEA 2020.1.3/lib/httpclient-4.5.12.jar;C:/Program Files/DevTools/IntelliJ IDEA 2020.1.3/lib/plexus-utils-3.2.0.jar;......
11868 Jps -m

PS D:\MyProject\IdeaProjects\java> jps -v
11904 XX -Xmx200m -javaagent:C:\Program Files\DevTools\IntelliJ IDEA 2020.1.3\lib\idea_rt.jar=57118:C:\Program Files\DevTools\IntelliJ I
DEA 2020.1.3\bin -Dfile.encoding=UTF-8
```

main 函数 args 参数中 args[0] 就是第一个参数。
与 C 的命令行参数不同，在 C 的 main 函数中 argv[0] 是程序名。

```java
public static void main(String[] args) {
    for (int i = 0; i < args.length; i++) {
        System.out.print(args[i] + " ");
    }
}
```
```shell
java jvm.XX onw two three
onw two three
```

```c
#include <stdio.h>
int main(int argc, char *argv[])
{
    for (int i = 0; i < argc i++) {
        printf("%s ", argv[i]);
    }
}
```
```shell
./a.out onw two three
./a.out onw two three
```

### 8.2.2. jstat 虚拟机统计信息监视工具

监视虚拟机各种运行状态信息（包括：类装载、内存、垃圾收集、JIT编译等运行数据）。是运行期定位虚拟机性能问题的首选工具。

jstat -gc 456 250 10
查询 VMID 为 456 的进程的 GC 情况，每 250 毫秒查询一次，共查询 10 次。

```shell
jstat -class 456
Loaded  Bytes  Unloaded  Bytes     Time
   625  1285.7        0     0.0       0.11

jstat -gc 456
 S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU    CCSC   CCSU   YGC     YGCT    FGC    FGCT     GCT
8192.0 8192.0  0.0    0.0   51712.0   5177.1   136704.0     0.0     4480.0 780.9  384.0   76.6       0    0.000   0      0.000    0.000
```

### 8.2.3. jinfo Java 配置信息工具

查看虚拟机参数配置

```shell
jinfo -flag MetaspaceSize 15048
-XX:MetaspaceSize=21807104
```

### 8.2.4. jmap Java 内存映像工具

用于生产堆转储快照（heapdump 或 dump 文件）。
```shell
jmap -dump:live,file=dump_001.bin 1344

Dumping heap to D:\MyProject\IdeaProjects\java\dump_001.bin ...
Heap dump file created

```

还可以使用-XX:+HeapDumpOnOutOfMemoryError参数来让虚拟机出现OOM的时候自动生成dump文件

### 8.2.5. jhat 堆转储快照分析工具

用于分析 jmap 生成的转储快照。

```shell
> jhat .\java_pid14596.hprof
Reading from .\java_pid14596.hprof...
Dump file created Wed Sep 16 14:58:37 GMT+08:00 2020
Snapshot read, resolving...
Resolving 13125 objects...
Chasing references, expect 2 dots..
Eliminating duplicate references..
Snapshot resolved.
Started HTTP server on port 7000
Server is ready.
```
浏览器输入 http://localhost:7000 访问分析结果


### 8.2.6. jstack Java 堆栈跟踪工具

生成虚拟机当前时刻的线程快照。线程快照是当前虚拟机内每一条正在执行的方法的堆栈集合。

|参数|含义|
|:-:|:-:|
|-F|当正常输出的请求不被响应时，强制输出线程堆栈|
|-l|除堆栈外，显示关于锁的附加信息|
|-m|如果调用本地方法的话，显示C/C++的堆栈|

```shell
jstack 15508

"main" #1 prio=5 os_prio=0 tid=0x000001c89180f000 nid=0x411c waiting on condition [0x0000002e616fe000]
   java.lang.Thread.State: TIMED_WAITING (sleeping)
        at java.lang.Thread.sleep(Native Method)
        at java.lang.Thread.sleep(Thread.java:340)
        at java.util.concurrent.TimeUnit.sleep(TimeUnit.java:386)
        at jvm.XX.main(XX.java:18)

"VM Thread" os_prio=2 tid=0x000001c8a3f31800 nid=0x184c runnable

"GC task thread#0 (ParallelGC)" os_prio=0 tid=0x000001c891824800 nid=0x308 runnable

"GC task thread#1 (ParallelGC)" os_prio=0 tid=0x000001c891826000 nid=0x25c runnable

"GC task thread#2 (ParallelGC)" os_prio=0 tid=0x000001c891828000 nid=0x237c runnable

"GC task thread#3 (ParallelGC)" os_prio=0 tid=0x000001c891829800 nid=0x2d18 runnable

"VM Periodic Task Thread" os_prio=2 tid=0x000001c8a4ae3800 nid=0x373c waiting on condition

JNI global references: 12
```

### 8.2.7. jconsole 可视化监视与管理控制台

使用jconsole 命令运行可视化控制台。