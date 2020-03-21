- [1 单例模式](#1-单例模式)
    - [1.1 单例模式的两种实现](#11-单例模式的两种实现)
    - [1.2 懒汉式线程不安全问题](#12-懒汉式线程不安全问题)
    - [1.3 线程不安全的解决方法](#13-线程不安全的解决方法)

# 1 单例模式

## 1.1 单例模式的两种实现

单例设计模式：在一个项目中某个类自始至终只有一个实例化的对象。

单例模式设计主要有以下几个要点：

1. 私有化构造器。这是为了防止在类的外部通过 new 来实例化对象，防止多个对象实例出现。
2. 提供一个 public 方法来获取单例对象。如，```getInstance()``` 方法。
3. 类内部提供一个本类的引用。一是为了通过 ```getInstance()```方法 向外部提供单例对象，二是为了防止 GC 清理掉单例对象。

单例模式主要有两种：饿汉式、懒汉式。其中饿汉式是线程安全的；懒汉式是线程不安全的，但是延迟了对象创建时机，一定程度上节省了内存开销。

下面通过代码来看一下这两种单例模式：

```java
/* 饿汉式 */
class Singleton {
	// 1.私有化构造器
	private Singleton () {}
	// 2.类内部创建一个本类的对象
	private static Singleton t = new Singleton();
	// 3.提供一个方法来获取实例对象
	public static Singleton getInstance() {
		return t;
	}
}
```

我们知道，类并不是一开始就在 JVM 的内存中的，只有当我们第一次使用一个类时，这个类才会被加载进内存。

饿汉式的单例模式，当类被加载时，会在 JVM 的方法区中的创建 static 变量，并执行 new 操作，产生单例对象。因为在整个程序运行过程中，一个类的加载只会执行一次，所以 ```new Singleton() ```只会被执行一次，因此饿汉式是线程安全的。

```java
/* 懒汉式  */ 
class Singleton {
	// 1.私有化构造器
	private Singleton() {}
	// 2.类内部创建一个本类的对象
	private static Singleton t = null;
	// 3.提供一个方法来获取实例对象
	public static Singleton getInstance() {
		if(t == null) {
			t = new Singleton();
		}
		return t;
	}
}
```

饿汉式的单例模式是在类被加载时创建单例对象；而懒汉式将对象的创建推迟到了第一次获取单例对象时。这种特性和数据库中的懒加载模式，以及 C# 中 EF 框架的延迟加载一样，都是将特定动作推迟到需要时进行。

## 1.2 懒汉式线程不安全问题

以上懒汉式的代码在第一次创建单例对象时是线程不安全的。当有多个线程同时获取单例对象时，就会发生创建多个实例的情况。看下面的测试代码：

```java
/* 懒汉式 线程不安全测试 */
public class SingletonTest{
	public static void main(String[] args) {
		for (int i = 0; i < 100; i++) {
			new Thread(
				() -> {
					System.out.println(Singleton.getInstance());
				}	
			).start();
		}
	}
}
```

运行结果：

![image-20200315192653309](images/image-20200315192653309.png) 

可以看到，当第一次获取单例对象时，就会出现多个线程同时进入临界区的情况，导致创建了多个实例对象。而在之后便不会发生，因为直接走了```return t```。

## 1.3 线程不安全的解决方法

（1）最直接的解决方法是在方法上加同步关键字 **synchronized** ：

```java
public static synchronized Singleton getInstance() {
	if(t == null) {
		t = new Singleton();
	}
	return t;
}
```

这样虽然解决了线程不安全的问题，但是带来了同步的性能问题。实际上，只有在第一次创建对象时才会有线程不安全的问题，之后获取单例对象并不需要同步。

（2）也就是说，不是整个方法都是临界区，仅仅是创建对象的那条语句才是临界区。

```java
/* 注意：仅仅这样并不能解决线程不安全 */
public static synchronized Singleton getInstance() {
    if(t == null) {
        synchronized (Singleton.class) {  
            singleton = new Singleton(); /* 注意：仅仅这样并不能解决线程不安全 */
        }
    }
    return t;
}
```

那么，给 new 语句加上同步代码块就可以解决线程不安全问题了吗？**答案是：并不能！**

可以想象一些，当第一次有 n 个线程同时要获取单例对象时，这 n 个线程都进入了 if 语句块，之后能够进入 synchronized 语句块的只有第一个线程；其他线程都进入阻塞状态。当第一个线程创建并获取完对象离开后，第二个线程被唤醒，于是它便**进入临界区 (注意：该线程是在进入临界区之前被阻塞的，已经过了 if 判断)**，继续执行创建对象的代码。之后被唤醒的线程都会这样做。

从上面的分析中，不难发现，在临界区中少了一个条件判断，用于区分第一个线程和阻塞队列中的线程。所以在加上一个判断条件就可以了。

```java
public static synchronized Singleton getInstance() {
    if(t == null) {
        synchronized (Singleton.class) {  
            if (singleton == null) {  
                singleton = new Singleton(); 
            }  
        }
    }
    return t;
}
```

上述代码中，第一个 if 判断是用来区别第一批获取对象的线程(第一批是单例对象没有被创建，需要执行 new 代码)和之后获取单例对象的线程；第二个 if 判断是用来区别在第一批线程中，第一个线程和之后在阻塞队列中的线程。就是说我们需要保证只有真正意义上的第一个线程才能执行创建对象代码。

（3）还有一种方法来实现线程安全

```java
public class Singleton {  
    private static class Inner {  
       private static final Singleton INSTANCE = new Singleton();  
    }  
    private Singleton (){}  
    public static final Singleton getInstance() {  
       return Inner.INSTANCE;  
    }  
}
```

上面代码里的这种方法，在```Singleton```类中定义了一个内部类```Inner```，内部类和类一样只有在使用的时候才被加载。当第一次使用```Inner.INSTANCE```时创建单例对象。内部类的创建方法则和饿汉式一样是线程安全的，外部类的```getInstance()```方法并不自己创建单例对象，它将这个任务委托给内部类来完成。这种方法没有使用 synchronized 语句，不受同步带来的性能影响。