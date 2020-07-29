<h1 align="center">Java 基础编程</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1 Java 简介](#1-java-简介)
  - [1.1 Java语言版本迭代](#11-java语言版本迭代)
  - [1.2 JDK JRE JVM 三者间的关系](#12-jdk-jre-jvm-三者间的关系)
  - [1.3 Hello World 程序](#13-hello-world-程序)
  - [1.4 注释](#14-注释)
- [2 变量](#2-变量)
  - [2.1 变量的声明和赋值](#21-变量的声明和赋值)
  - [2.2 Java 变量的分类](#22-java-变量的分类)
    - [2.2.1 按数据类型分](#221-按数据类型分)
    - [2.3.2 按声明位置分](#232-按声明位置分)
  - [2.3 变量间的运算](#23-变量间的运算)
    - [2.3.1 基本数据类型间的运算（不包括boolean）](#231-基本数据类型间的运算不包括boolean)
    - [2.3.2 基本数据类型和String间的运算](#232-基本数据类型和string间的运算)
- [3 标识符和关键字](#3-标识符和关键字)
  - [3.1 关键字和保留字](#31-关键字和保留字)
  - [3.2 标识符](#32-标识符)
- [4 运算符](#4-运算符)
  - [4.1 算术运算符](#41-算术运算符)
  - [4.2 赋值运算符](#42-赋值运算符)
  - [4.3 关系运算符（比较运算符）](#43-关系运算符比较运算符)
  - [4.4 逻辑运算符](#44-逻辑运算符)
  - [4.5 位运算符](#45-位运算符)
    - [4.5.1 进制](#451-进制)
    - [4.5.2 原码 反码 补码](#452-原码-反码-补码)
    - [4.5.3 位运算](#453-位运算)
  - [4.6 三元运算符](#46-三元运算符)
  - [4.7 运算符的优先级](#47-运算符的优先级)
- [5 数组](#5-数组)
- [6 面向对象编程基础](#6-面向对象编程基础)
  - [6.1 包（package与import）](#61-包package与import)
  - [6.2 访问权限修饰符](#62-访问权限修饰符)
  - [6.3 this](#63-this)
  - [6.4 super](#64-super)
  - [6.5 static](#65-static)
    - [6.5.1 static修饰变量：类变量](#651-static修饰变量类变量)
    - [6.5.2 static修饰方法：静态方法](#652-static修饰方法静态方法)
    - [6.5.3 类加载的过程](#653-类加载的过程)
  - [6.6 final](#66-final)
  - [6.7 JavaBean](#67-javabean)
- [7 类的成员](#7-类的成员)
  - [7.1 属性 Field](#71-属性-field)
  - [7.2 方法 Method](#72-方法-method)
    - [7.2.1 方法的参数传递：值传递](#721-方法的参数传递值传递)
    - [7.2.2 方法重载 overload](#722-方法重载-overload)
    - [7.2.3 方法重写 override](#723-方法重写-override)
    - [7.2.4 方法重载与方法重写的区别](#724-方法重载与方法重写的区别)
    - [7.2.5 可变形参](#725-可变形参)
  - [7.3 构造器 Constructor](#73-构造器-constructor)
  - [7.4 代码块](#74-代码块)
  - [7.5 内部类](#75-内部类)
    - [7.5.1 匿名子类（匿名内部类）](#751-匿名子类匿名内部类)
- [8 面向对象三大特征：封装 继承 多态](#8-面向对象三大特征封装-继承-多态)
  - [8.1 封装性](#81-封装性)
  - [8.2 继承性](#82-继承性)
  - [8.3 多态性](#83-多态性)
  - [8.4 面向对象的第四大特征：抽象性](#84-面向对象的第四大特征抽象性)
    - [8.4.1  模板方法(TemplateMethod)设计模式](#841-模板方法templatemethod设计模式)
- [9 Java 常用类](#9-java-常用类)
  - [9.1 Object 类](#91-object-类)
    - [9.1.1 equals方法](#911-equals方法)
    - [9.1.2 hashCode方法](#912-hashcode方法)
    - [9.1.3 toString方法](#913-tostring方法)
    - [9.1.4 clone方法](#914-clone方法)
- [10 接口](#10-接口)

<!-- /code_chunk_output -->

# 1 Java 简介

## 1.1 Java语言版本迭代

1995年发布JDK1.0；JDK1.5开始改名为JDK5.0,以后都是6.0，7.0 …。

JavaSE（J2SE）：Java核心API + 桌面级应用开发

JavaEE（J2EE）：企业级开发（Web应用）

JavaME（J2ME）：移动端应用开发

## 1.2 JDK JRE JVM 三者间的关系

JDK = JRE + Java开发工具集

JRE = JVM + Java SE 标准类库

## 1.3 Hello World 程序

(1) **创建**：创建一个以.java结尾的文件

(2) **编辑**：

``` java  
public class HelloWorld {    
    public  static void main(String[] args) {        
        System.out.println("hello, world");    
    }  
} 
```

(3) **编译**：javac 源文件名.Java     可以生成一个或多个字节码文件（有几个类就有几个字节码文件）

(4) **运行**：java 字节码文件名（注意：不要.class拓展名）

说明：

1. 一个源文件中可以由多个类，但是只能有一个public类；

2. public类的名字必须和源文件名一样

## 1.4 注释

(1) **单行注释**：//

(2) **多行注释**：/* */

​    多行注释不能被嵌套使用

(3) **文档注释**：/** */

javadoc -d 目标路径 -author -version 源文件名.java



# 2 变量

## 2.1 变量的声明和赋值

第一种：声明变量，同时赋值

```java
// 一个变量  
int a = 1;  
// 多个变量  
int a = 1, b = 2, c = 3;  
```

第二种：先声明后赋值

```java
// 一个变量  
int a;  
a = 2;  
// 多个变量  
int a, b, c;  
a = b = c = 10;  
```

说明：

1.  变量的作用域：声明变量所在的那对大括号内；

2.  在同一个作用域内，变量命不允许重复；

3.  变量应该先声明后使用；

4.  同一个变量可以被多次赋值，后一次赋值覆盖前一次赋值。

## 2.2 Java 变量的分类

### 2.2.1 按数据类型分

(1) 整数类型：byte、short、int、long

​	Java的整数类型有固定的表数范围和字段长度，不受具体的操作系统的影响，以保证Java程序的可移植性。

​	Java整型常量默认为int类型，long类型常量必须在末尾加上“l”或者“L”。

| **类型** | **占用的存储空间** | **表数范围**             |
| -------- | ------------------ | ------------------------ |
| byte     | 1字节              | -128 ~ 127               |
| short    | 2字节              | -32768 ~ 32767           |
| int      | 4字节              | -2147483648 ~ 2147483647 |
| long     | 8字节              | -2^63  ~ 2^63-1          |

(2) 浮点类型：float、double

​	Java的浮点类型有固定的表数范围和字段长度，不受具体的操作系统的影响，以保证Java程序的可移植性。

​	Java浮点型常量默认为double类型（也可以在末尾加上“d”或者“D”），float类型常量必须在末尾加上“f”或者“F”。

​	浮点数表示形式：

​		十进制数表示法：5.12，512.0f，.512

​		科学计数表示法：5.12e2，512E2，100E-2

| **类型** | **占用的存储空间** | **表数范围**           |
| -------- | ------------------ | ---------------------- |
| float    | 4字节              | -3.403E38 ~ 3.403E38   |
| double   | 8字节              | -1.798E308 ~ 1.798E308 |

(3) 字符类型：char 

​	Java中的字符类型是占两个字节的。其表示方式有以下三种：

```java
// 单个普通字符  
char c1 = 'A';
char c2 = '中'; 
// 转义字符  
char n1 = '\n';  
// 3. Unicode值  
char n2 = '\u0056';
char c3 = 65;  
```

| 转义字符 | 意义                                | ASCII码值（十进制） |
| -------- | ----------------------------------- | ------------------- |
| \a       | 响铃(BEL)                           | 007                 |
| \b       | 退格(BS) ，将当前位置移到前一列     | 008                 |
| \f       | 换页(FF)，将当前位置移到下页开头    | 012                 |
| \n       | 换行(LF) ，将当前位置移到下一行开头 | 010                 |
| \r       | 回车(CR) ，将当前位置移到本行开头   | 013                 |
| \t       | 水平制表(HT) （跳到下一个TAB位置）  | 009                 |
| \v       | 垂直制表(VT)                        | 011                 |
| \\       | 代表一个反斜线字符''\'              | 092                 |
| \'       | 代表一个单引号（撇号）字符          | 039                 |
| \"       | 代表一个双引号字符                  | 034                 |
| \?       | 代表一个问号                        | 063                 |
| \0       | 空字符(NUL)                         | 000                 |
| \ddd     | 1到3位八进制数所代表的任意字符      | 三位八进制          |
| \xhh     | 十六进制所代表的任意字符            | 十六进制            |

**拓展:Unicode编码与UTF-8**

**UTF-8** **是 Unicode 的实现方式之一。**

UTF-8 的编码规则很简单，只有二条：

(1) 对于单字节的符号，字节的第一位设为0，后面7位为这个符号的 Unicode 码。因此对于英语字母，UTF-8 编码和 ASCII 码是相同的。

(2) 对于n字节的符号（n > 1），第一个字节的前n位都设为1，第n + 1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的 Unicode 码。

![image-20200319153726395](../images/image-20200319153726395.png) 

跟据上表，解读 UTF-8 编码非常简单。如果一个字节的第一位是0，则这个字节单独就是一个字符；如果第一位是1，则连续有多少个1，就表示当前字符占用多少个字节。

引用自: http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html

(4) 布尔类型

只有true和false两个值。

(5)  各类型所占字节数

```java
/**
输出Java中基本类型所占字节数
版权声明：本文为CSDN博主「阳光岛主」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/ithomer/article/details/7310008
*/
public class CalSize {
    public static void main(String[] args) {
		System.out.println("int:\t" + Integer.SIZE/8);      // 4
		System.out.println("short:\t" + Short.SIZE/8);      // 2	
		System.out.println("long:\t" + Long.SIZE/8);	    // 8
		System.out.println("byte:\t" + Byte.SIZE/8);        // 1
		System.out.println("char:\t" + Character.SIZE/8);   // 2
		System.out.println("float:\t" + Float.SIZE/8);      // 4
		System.out.println("double:\t" + Double.SIZE/8);    // 8
		//System.out.println("Boolean: " + Boolean);
    }
}
```

![image-20200319153805485](../images/image-20200319153805485.png) 

### 2.3.2 按声明位置分

（1）成员变量

（2）局部变量

![image-20200319154014930](../images/image-20200319154014930.png) 

## 2.3 变量间的运算

### 2.3.1 基本数据类型间的运算（不包括boolean）

**(1) 自动类型提升**

自动类型提升：小容量的变量与大容量的变量做运算，结果用大容量的变量来接收。

byte，short，char --> int --> long --> float --> double

注意：

1、容量指的是表数范围，而不是所占存储空间。float（4字节）的表数范围大于long（8字节）。

​       2、byte，short，char三者之间的运算都会自动提升为int类型。

![image-20200319154117223](../images/image-20200319154117223.png) 

**这是字段类型提升的一个坑：**

自动类型提升有好处，但它也会引起令人疑惑的编译错误。例如，下面看起来正确的程序却会引起问题：

```java
byte b = 50;
b = b * 2;    // Type mismatch: cannot convert from int to byte
```

如上所示，第二行会报“类型不匹配：无法从int转换为byte”错误。

![image-20200319154221155](../images/image-20200319154221155.png) 

该程序试图将一个完全合法的 byte 型的值 50*2 再存储给一个 byte 型的变量。但是当表达式求值的时候，操作数被自动的提升为 int 型，计算结果也被提升为 int 型。这样表达式的结果现在是 int 型，不强制转换它就不能被赋为 byte 型。确实如此，在这个特别的情况下，被赋的值将仍然适合目标类型。

所以应该使用一个显示的强制类型转换，例如：

```java
byte b = 50;
b = (byte)(b * 2);
```

这样就能产生正确的值 100。

注意：char 类型比较特殊，char 自动转换成 int、long、float 和 double，但 byte 和 short 不能自动转换为 char，而且 char 也不能自动转换为 byte 或 short。

引用自：http://c.biancheng.net/view/796.html

**这是另一个坑**

float和long运算，会自动类型提升为float。例如如下代码编译错误：

```java
long l = 200L;
float f = 10.0F;
long r = l + f;
```

![image-20200319154315289](../images/image-20200319154315289.png) 

需要使用float类型变量来存储运算结果。

```java
long l = 200L;
float f = 10.0F;
float r = l + f;
```

**(2) 强制类型转换：自动类型提升的逆过程。**

格式：目标类型 变量 = (目标类型)源类型变量/常量

注意：

​	1、使用强制类型转换运算符号“(类型)”。

​	2、可能会损失精度或者类型溢出。

​	3、在强制类型转换中目标类型和源类型变量的类型始终没有发生改变。

### 2.3.2 基本数据类型和String间的运算

​	1、 字符串和基本数据类型之间只能做连接运算，没有自动类型提升。

​	2、 字符串做链接运算的结果是字符串类型，只能用字符串来接受

​	3、 注意“+”号是做加法，还是做字符串连接符。

# 3 标识符和关键字

## 3.1 关键字和保留字

关键字的定义：Java关键字是Java语言里事先定义的，被赋予了特殊含义的标识符。

关键字的特点：关键字均为小写。

保留字的定义：现版本暂未使用，但以后版本可能用来作为关键字。

![image-20200321192912582](../images/image-20200321192912582.png)

![image-20200321192932199](../images/image-20200321192932199.png)

注：java官方文档有说明: “An identifier cannot have the same spelling (Unicode character sequence) as a keyword (§3.9), boolean literal (§3.10.3), or the null literal (§3.10.7), or a compile-time error occurs. While true and false might appear to be keywords, they are technically boolean literals (§3.10.3). Similarly, while null might appear to be a keyword, it is technically the null literal (§3.10.7). ”

【译文: 标识符不能具有与关键字（§3.9），布尔文字（§3.10.3）或空文字（§3.10.7）相同的拼写（Unicode字符序列），否则会发生编译时错误。 尽管true和false可能是关键字，但从技术上讲，它们是布尔文字（第3.10.3节）。 同样，尽管null似乎是一个关键字，但从技术上讲，它是null文字（第3.10.7节）。】

true false null不属于关键字，但是属于标识符。规定的关键字只有50个，包含两个保留字(goto，const)，但是这53个都属于标识符。它们之间的关系是：标识符包含 关键字、boolean literal（true,false）、 null literal; 关键字里面又包含有两个保留字。

引用自：https://blog.csdn.net/u012506661/article/details/52756452下的评论

以下是《Java 语言规范 基于 Java SE 8》一书中关于“关键字”的描述。（第15页）

![image-20200321193000464](../images/image-20200321193000464.png) 

## 3.2 标识符

合法标识符：

​	1、 由大小写英文字母（a-z, A-Z），数字（0-9），下划线（_），美元符号（$）组成；

​	2、 不能以数字开头；

​	3、 不可以使用关键字、保留字、true、false、null

​	4、 标识符严格区分大小写，长度无限制

​	5、 标识符不能包含空格

标识符命名风格：

1.【强制】代码中的命名均不能以下划线或美元符号开始，也不能以下划线或美元符号结束。

2.【强制】代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式

3.【强制】类名使用UpperCamelCase风格，但以下情形例外：DO / BO / DTO / VO / AO / PO / UID等。

4.【强制】方法名、参数名、成员变量、局部变量都统一使用lowerCamelCase风格，必须遵 从驼峰形式

5.【强制】常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字 长。

6.【强制】抽象类命名使用Abstract或Base开头；异常类命名使用Exception 结尾；测试类 命名以它要测试的类的名称开始，以Test结尾

7.【强制】包名统一使用小写，点分隔符之间有且仅有一个自然语义的英语单词。包名统一使 用单数形式，但是类名如果有复数含义，类名可以使用复数形式。

8.【参考】枚举类名带上Enum后缀，枚举成员名称需要全大写，单词间用下划线隔开。

引用自：《Java 开发手册》——阿里巴巴

![image-20200321193059390](../images/image-20200321193059390.png) 

图片引用自：https://zhuanlan.zhihu.com/p/96100037

单下划线作为标识符是不可行的，但是双下划线是可行的。

```java
public class _ {
    public static void main(String[] args) {
        int _ = 99;
        System.out.println(_);
    }
}
```

![image-20200321193132879](../images/image-20200321193132879.png) 

# 4 运算符

## 4.1 算术运算符

![image-20200321193506653](../images/image-20200321193506653.png)

注意：

​		1、整数除法的结果是整数，即求整除的商。

​		2、取余运算只适用于两个整数，结果的正负号和被取余数的正负号相同。

## 4.2 赋值运算符

=、+=、-=、*=、/=、%=、<<=、>>=、&=、^=、|=

设byte b = 10;，b += 2并不等于 b = b + 2。后者会进行数据类型提升，而前者数据类型不发生转换。

设int i = 1;，i *= 0.1可以相当于i = (int)(i * 0.1)。结果为0。上面的b += 2可以相当于b = (byte)(b + 2)。

```java
public class SetValue {
    public static void main(String[] args) {
        byte b = 10;
        b += 2;  // 不会进行类型转换
        System.out.println(b);
        
        b = b + 2; // 报错：类型不兼容
        System.out.println(b);

int i = 1;
        i *= 0.1; // 0，相当于 i = (int)(i * 0.1)
        System.out.println(i);
    }
}
```

【面试题】

![image-20200321193554367](../images/image-20200321193554367.png) 

答：1 编译不通过，因为short在做运算时，会先自动类型提升为int类型，所以必须用int类型的变量来接收结果。2 编译通过，s的值变为5。+=不会改变原来的数据类型，相当于 s = (short)(s+2)。

## 4.3 关系运算符（比较运算符）

![image-20200321193702557](../images/image-20200321193702557.png)

关系运算符的结果都是boolean类型的，值要么是true，要么是false。

这里说明一下，Java和C/C++不一样，在Java中if语句的小括号里面的值或者表达式的值必须为boolean类型，否则会报错：类型不兼容。因此如果是一条int类型的赋值语句，则不能作为if的条件（在C/C++中是可以的）。但需要注意的是如果赋值表达式的左值是boolean类型的，那么赋值表达式的值也会是boolean类型的。代码如下：

```java
public class Equals {
    public static void main(String[] args) {
        boolean boo = false;

        // 这里不会报错， 因为赋值语句的值是表达式最左边变量的值
        // 即就是boo的值，是boolean类型的
        if(boo = true)
        {
            System.out.println("第一个if里面的语句执行了");
        }

        int number = 0;
        // 这里会报错：类型不兼容。因为表达式number = 3的值是number的值3
        // 而if里面所需要的值必须为boolean类型
        if(number = 3)
        {
            System.out.println("第二个if里面的语句执行了");
        }

        System.out.println("完成！");
    }
}
```

## 4.4 逻辑运算符

& 逻辑与   | 逻辑或  ! 逻辑非

&& 短路与  || 短路或  ^ 逻辑异或

![image-20200321193755421](../images/image-20200321193755421.png)

异或：异为真。不一样，则结果为真。

说明：逻辑运算的操作数都是boolean类型，运算结果也是boolean类型。



【面试题】 &与&&，|与||的区别。

答：

![image-20200321193903747](../images/image-20200321193903747.png) 

```java
public class Logic {
    public static void main(String[] args) {
        int x, y;
        x = 0; y = 10;
        // 先判断x==0, 0==0为true，再执行x++, x=1；
        // 前面的为true，根据短路性，后面的不执行，y=10。
        if( (x++ == 0) || ((y=20) == 20) )
        {
            System.out.println("y="+y);
        }
        
        // 先执行x++, x=1；再判断x==0, 1==0为false；
        // 前面的为false，不能直接推断出结果，执行后面的语句，y=20。
        if( (++x == 0) || ((y=20) == 20) )
        {
            System.out.println("y="+y);
        }
        
        int a, b;
        a = 0; b = 10;
        // 先判断x==0, 0==0为true，再执行x++, x=1；
        // 前面的为true，没有短路性，执行后面的语句，y=20。
        // 或者把 | 理解为位运算操作符。
        if( (a++ == 0) | ((b=20) == 20) )
        {
            System.out.println("b="+b);
        }
    }
}
```

结果：

![image-20200321193935285](../images/image-20200321193935285.png) 

这篇文章对&和|的理解思路很清奇，认真看完，不要看到第一个问题就不看了。文章地址：https://blog.csdn.net/websph/article/details/5669363

## 4.5 位运算符

### 4.5.1 进制

世界上有10种人，一种是都二进制的，另一种是不懂二进制的。

l 二进制：0, 1，以0b或者0B开头；

l 十进制：0-9；

l 八进制：0-7，以0开头；

l 十六进制：0-9及A-B，以0x或者0X开头。A-F大小写不区分。

### 4.5.2 原码 反码 补码

(1)  正数的原码、反码、补码都相同。（9的原码：0000 1001）

(2)  负数

负数的原码：最高位为符号位，1表示负数。即把其对应的正数的原码的符号位改为1。（-9的原码：1000 1001）

负数的反码：符号位不变，把负数的原码的其它位按位取反。（-9的反码：1111 0110）

负数的补码：反码加一。（-9的补码：1111 0111）

 

(byte)128 => -128

128：0000 0000, 0000 0000, 0000 0000, 1000 0000

(byte)128：1000 0000 => -128

### 4.5.3 位运算

Java定义了位运算符，应用于整数类型(int)，长整型(long)，短整型(short)，字符型(char)，和字节型(byte)等类型。位运算符作用在所有的位上，并且按位运算。

![image-20200321194105934](../images/image-20200321194105934.png)

<< 左移： 规则是带符号位移，高位移出，低位补0，移动位数超过该类型的最大位数，则进行取模，如对Integer型左移34位，实际上只移动了两位。左移一位相当于乘以2的一次方，左移n位相当于乘以2的n次方。

\>> 右移：规则是低位移出，高位补符号位，移动位数超过该类型的最大位数，则进行取模，如对Integer型左移34位，实际上只移动了两位。

\>>> 无符号右移：无符号位移是什么意思呢，就是右移的时候，无论正负数，高位始终补0。当然，它也仅仅针对负数计算有意义。

 

直接上代码：

```java
public class Bit {
    public static void main(String[] args) {
        byte a = -1; // 1111 1111
        
        byte b1 = (byte)(a >> 4);
        byte b2 = (byte)(a >>> 4);
        
        System.out.println(b1); // -1
        System.out.println(b2); // -1
        // byte类型运算时会转为int类型，后强转为byte后只有低8位，全为1。
        
        System.out.println("-------------------");
        
        int i = -1; // 1111 1111
        
        // 高位补符号位 1111 1111 1111 1111, 1111 1111 1111 1111
        int c1 = i >> 4;
        // 高位补零 0000 1111 1111 1111, 1111 1111 1111 1111  ==  ‭268435455‬‬‬‬‬‬‬‬‬‬‬‬‬‬‬‬‬‬‬‬‬‬‬‬‬
        int c2 = i >>> 4; 
        
        System.out.println(c1);
        System.out.println(c2);
    }
}
```

![image-20200321194145034](../images/image-20200321194145034.png) 

这个有意思哦~ 并不是循环左移

```java
System.out.println(1 << 31);
System.out.println(3 << 31);
System.out.println(3 << 32);
System.out.println(3 << 33);
System.out.println((3 << 31) << 1); // 这个好玩
System.out.println(-1 << 32);
System.out.println(-1 << 33);
System.out.println(-1 << 34); // 得-4 不是循环左移
```

![image-20200321194218013](../images/image-20200321194218013.png) 

## 4.6 三元运算符

(条件表达式) ? 表达式1 : 表达式2

​		1、条件表达式的结果为boolean类型。

​		2、条件表达式为true，执行表达式1；为false, 执行表达式2。

​		3、表达式1和表达式2的类型必须相同。或者可以进行类型转换。

​		4、三元运算符可以被嵌套使用，但不建议这样使用。

​		5、三元运算符可以被if语句替代；反之，不成立。

## 4.7 运算符的优先级

![image-20200321194333677](../images/image-20200321194333677.png)



# 5 数组

数组：相同数据类型的变量的集合。

​	1、在Java中数组本身属于引用数据类型。

​	2、数组中的元素既可以是基本数据类型，也可以是引用数据类型。

​	3、**Java的数组都是分配在堆上的。**

​	4、数组创建完成后，长度不可以改变。

数组的声明和初始化：

```java
// 数组的声明
String[] name;
int cs[]; // 不建议这种的
// 静态初始化
name = new String[]{"1", "2", "3", "4"};
int numbers[] = {1, 2, 3, 4};
// 动态初始化
cs = new int[5];
double d[] = new double[10];
```

数组元素的默认值：

- byte、short、int、long = 0
- float、double = 0.0
- boolean = false
- char = \u0000
- 引用类型 = null

数组的属性length用来表示数组长度。

数组的下标从0开始到 (length - 1) 结束。

```java
int[] arr = new int[10];
for (int i = 0; i < arr.length; i++) {
	arr[i] = i;
}
for (int i = 0; i < arr.length; i++) {
	System.out.println(arr[i]);
}
```

- 数组下角标异常：ArrayIndexOutOfBoundsException
- 空指针异常：NullPointerException

Arrays 类

```java
Arrays.sort();         // 排序
Arrays.toString();     // 数组转字符串
Arrays.binarySearch(); // 二分查找
Arrays.fill();         // 内容填充
```



# 6 面向对象编程基础

类是对象的抽象，对象是类的实例。

类的成员：属性、方法、构造器、代码块、内部类。

面向对象三大特征：封装性、继承性、多态性。

面向过程强调的是功能行为；面向对象强调的是具备了功能的类。

匿名对象：new Object()，一般用于方法的实参。

## 6.1 包（package与import）

包的作用：为了对Java类进行统一的管理。

包命名规范：包名统一使用**小写**，点分隔符之间有且仅有一个自然语义的英文单词或者多个单词自然连接到一块（如 springframework，deepspace不需要使用任何分割）。包名统一使用**单数形式**，如果类名有复数含义，则可以使用复数形式。包名的构成可以分为以下几四部分【前缀】【发起者名】【项目名】【模块名】。常见的前缀可以分为以下几种：

![image-20200328142727668](../images/image-20200328142727668.png) 

每个“.”代表一层目录。

不同的包下面可以有相同的类名，同一个包下不能有相同的类名。（虽然Java严格区分大小写，同一个包下可以有DbContent和dbcontent；但是文件系统不区分大小写，导致文件名重复：Type with same name but different case exists.【具有相同名称但大小写不同的类型。】）

package位于源文件的首行，即package语句是代码的第一行非空行。

import：

1. 位于package语句后面，类的定义前面。

2. 用于显示地导入指定包下的类或者接口。

3. 如果需要导入多个类或者接口，则需要并列地写多条import语句。

	4. 也可以使用“import java.util.*;”这种方式导入该包下的所有类或接口。
 	1. 但是不会导入子包，例如：import java.util.*; 可以导入Scanner类，而import java.*;不能。
 	2. 这种方法并不会导入所有的类，只会导入用到的类，编译时会替换成单类型导入。

5. 已经导入了一个包，如果还需要它的子包，那么子包还需另外导入。

6. java.lang包下的类和接口是默认自动导入的,当前包的成员本身就在作用域内所以当前包和java.lang包的导入是可以省略的。

7. 如果有使用到不同包下的同名类，只能使用类的全名来指明。

8. import static 调用指定包下指定类或接口中的静态属性和方法。

``` java
import static java.lang.System.out;
out.println("hello");
```

参考：https://blog.csdn.net/qq_25665807/article/details/74747868

## 6.2 访问权限修饰符

对于class的访问权限只能用public和default（缺省）来修饰：

​    public：任何地方可以访问该类；

​    default：只有在同一个包下访问该类。

代码块只能用static修饰。

| **修饰符** | **类内部** | **同一个包** | **不同包的子类** | **任何地方** |
| :--------: | :--------: | :----------: | :--------------: | :----------: |
|  private   |     √      |              |                  |              |
|  （缺省）  |     √      |      √       |                  |              |
| protected  |     √      |      √       |        √         |              |
|   public   |     √      |      √       |        √         |      √       |

## 6.3 this

this表示当前对象。可以用来使用类的属性、方法和构造器。

this.属性名：用来指定当前对象的属性，this在不引起歧义时可以省略。

this.方法名()：用来调用当前对象的非静态方法，this在不引起歧义时可以省略。

this()用于调用类的其他构造器。可以多级调用，但禁止套娃。this()必须放在构造器的首行，一个构造器中只能出现一个this()。 à 推论：n个构造器中一共最多可出现n-1条this()语句。

this不能出现在静态上下文中。

## 6.4 super

super表示当前对象的父类。可以用来使用父类的属性、方法和构造器。

当父类中的方法被子类重写后，在子类中可以使用【super.方法名】来调用父类的被重写方法，使用【this.方法名】来调用子类的重写方法。如果子类没有重写，那么【super.方法名】和【this.方法名】都是调用父类的方法，或者直接使用【方法名】（省略了this.）。属性同理。

如果有一个方法继承时一路重写下来，那么【super.方法名】调用的是直接父类的方法。

super()用来调用父类的构造器。在子类的构造器中，如果没有显示地使用super()调用父类构造器，或者使用this()来调用其他构造器，那么Java默认自动的调用父类的无参构造器。如果父类没有无参构造器，就会报错【Implicit super constructor Person() is undefined. Must explicitly invoke another constructor】。（这也就解释了为什么Java会白送一个无参构造器）

super()必须放在子类构造器的首行，一个构造器中只能出现一个super()。

创建子类对象必定会调用父类构造器。

![image-20200328143202169](../images/image-20200328143202169.png) 

## 6.5 static

static可以用来修饰：属性、方法、代码块、内部类。

### 6.5.1 static修饰变量：类变量

1、同一个类的所有对象共同拥有(共享)一份类变量；每个对象各自拥有一份实例变量。

2、类变量随着类的加载而创建；实例变量随着对象的创建而创建。

3、在程序的一次运行过程中，每个类只会被加载一次。

4、使用类变量：类名.类变量名 / 对象名.类变量名。

### 6.5.2 static修饰方法：静态方法

1、静态方法随着类的加载而加载。

2、调用静态方法：类名.静态方法名(参数列表) / 对象名.静态方法名(参数列表)。

3、静态方法中不能使用非静态的实例变量，可以使用静态的类变量。静态方法中不能调用非静态的方法，可以调用静态方法。

4、非静态方法中可以使用类变量和静态方法。

5、静态方法中不能使用“this”和“super”。



【思考】什么时候使用static修饰属性和方法？

static修饰属性：

​	1、当一个属性作为常量时，必须使用static修饰。

​	2、当多个对象共同使用一份属性时。

static修饰方法：

​	1、工具类中的方法一般使用static修饰。

​	2、有时为了使用类变量，方法也会使用static修饰。

### 6.5.3 类加载的过程

1、当我们创建对象时，首先会在方法区中查找该类的信息。

2、如果在方法区中没有找到该类的信息，则进行类加载。如果有，直接创建对象。

3、类加载：将字节码文件加载到JVM中，同时在方法区的特定区域中存放static变量。（细节：不是只有创建对象时才进行类加载，而是当我们调用类时就进行类加载）

![image-20200328143326865](../images/image-20200328143326865.png) 

## 6.6 final

final修饰的类：不能被继承。如，String、StringBuffer。

final修饰的方法：不能被重写。

final修饰的属性：必须要且仅能被赋值一次(即初始化)，且只有显示赋值、代码块赋值、构造器赋值这三种方式。

final修饰的方法体/代码块的局部变量：必须要且仅能被赋值一次，可以声明的同时赋值，也可以先声明后赋值。

final修饰的方法的形参，保证参数不被改变（如果参数是引用类型，只保证引用不改变所指对象，而对象的属性的值可以改变）。

常量：可以通过类名调用，static；常量的值不可改变，final；名称全大写。

```java
public static final double PI = 3.14159265358979323846;
```

## 6.7 JavaBean

JavaBean是一种Java语言写成的可重用组件。

所谓JavaBean，是指符合以下标准的Java类：

- 类是具体的，且是public的；
- 有一个无参数的public的构造器；
- 有属性，且每一个属性具有对应的get、set方法。



# 7 类的成员

## 7.1 属性 Field

局部变量和成员变量的相同点和不同点：

​    **相同点：**

​		1、都是变量，都要先声明后使用
​		2、都可以被多次赋值
​		3、都有其作用域
​		4、声明方式：变量类型 变量名

​	**不同点：**

​		1、声明位置：
​			成员变量声明在类内部，方法、构造器等结构外部；
​			局部变量声明在方法中、方法的形参、构造器中、构造器的形成等。

​		2、默认值
​			成员变量默认值：
​			    byte、short、int、long = 0
​    			float、double = 0.0
​    			boolean = false
​    			char = \u0000
​    			引用类型 = null；

​			局部变量没有默认值。

​		3、权限修饰符
​			成员变量可以被权限修饰符修饰；
​			局部变量不能。

​		4、内存
​			成员变量存储在堆上；
​			局部变量存储在栈上。

## 7.2 方法 Method

```java
权限修饰符 返回值类型 方法名(参数列表) {
	方法体
}
```

1、方法可以被权限修饰符所修饰。
2、使用return语句来返回一个数据；在return 语句处结束方法，返回上一层调用。
3、调用方法：对象名/类名.方法名(实参列表)。
4、形参列表中多个参数用逗号（,）分开。

### 7.2.1 方法的参数传递：值传递

**形式参数**：方法声明时参数列表中的参数。
**实际参数**：方法调用时实际传递给方法的参数。

在Java中，方法的参数传递方式只有一种：值传递。即将实参的值复制一份传递到方法内，实参本身不受影响。按值传递重要特点：传递的是值的拷贝，也就是说传递后就互不相关了。

对于基本类型来说，传递的是实参的副本（值传递），故在方法内修改传递进来的值并不会影响实参本身；

对于引用类型来说，传递进来的是引用变量的副本（也是值传递），因此该副本与实参均是引用变量，他们均可以操作所引用的对象，在方法内通过引用变量对堆区的对象进行操作时均会对该对象有影响。由于传入方法的是实际参数值的副本，因此，参数本身不会受到任何影响！

### 7.2.2 方法重载 overload

**方法重载的定义**

方法重载：同一个类中，方法名相同，**形参列表不同**，则构成方法重载。

不同的形参列表指：形参类型不同，个数不同，顺序不同。

注意：

  	1、只有返回值不同，不是重载，会报错；
  	2、只有形参的变量名不同，不是重载，会报错；
  	3、只有权限修饰符不同，不是重载，会报错。

**方法重载时的调用选择**

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

​	(1)   精确匹配：对于上述代码中，当有method(int a)存在时调用的肯定就是这个方法；

​	(2)   自动类型提升：对于基础数据类型，自动转成表示范围更大的类型；当方法1被注释的时候，会去调用method(long a)而不是method(Integer a)；

​	(3)   自动装箱与拆箱：当方法1，2被注释，就调用方法3；

​	(4)   根据子类依次向上继承路线匹配：当只有方法4与方法5时，先找int的父类，找到的object类型，匹配之后调用；当继承A类之后，由于本类没有合适的方法，然后就去A类中找，匹配调用（A类中方法参数类型换成long，Integer结果也一样）；

​	(5)   根据可变参数匹配。

引用自：https://www.jianshu.com/p/306c4bfe3f54

### 7.2.3 方法重写 override

​		定义：在子类中可以根据需要对从父类中继承而来的方法进行改写，也称为方法的重置、覆盖。在程序执行时，子类的方法将覆盖掉父类的方法。

​		要求：

​	1、重写方法必须和被重写方法具有相同的方法名、形参列表。

​	2、重写方法不能使用比被重写方法更严格的访问权限。

​	3、重写方法的返回值类型要么和被重写方法一致，要么是被重写方法返回值类型的子类。如果是基本数据类型或者void，那么两者必须一样。

​	4、重写方法和被重写方法必须同时为非static的。如果同时为static的，不是方法重写。父类的静态方法可以被子类继承，但是不能重写，静态方法属于静态绑定。

​	5、重写方法抛出的异常不能大于被重写方法的异常。

​	6、父类中被private修饰的方法不可以被子类重写。如果子类中有相同的方法，则会被视作方法重载。

可以使用【@Override】注解来说明当前方法是一个重写方法。

Java 中其实没有虚函数的概念，它的普通函数就相当于 C++ 的虚函数，**动态绑定**是Java的默认行为。如果 Java 中不希望某个函数具有虚函数特性，可以加上 final 关键字变成非虚函数。

### 7.2.4 方法重载与方法重写的区别

**【方法重载】**：

​	1、同一个类中
​	2、方法名相同，参数列表不同（参数顺序、个数、类型）
​	3、方法返回值、访问修饰符任意
​	4、与方法的参数名无关

**【方法重写】**：

​	1、有继承关系的子类中
​	2、方法名相同，参数列表相同（参数顺序、个数、类型），方法返回值相同
​	3、访问修饰符，访问范围需要大于等于父类的访问范围
​	4、与方法的参数名无关

![image-20200403133223533](../images/image-20200403133223533.png)

### 7.2.5 可变形参

1、格式，对于方法的形参，数据类型… 可变形参的参数名。

2、可变形参方法与同名的方法构成重载，与形参是数组的方法不构成重载(会报错)。例如：int sum(int[] array)与int sum(int... array)只能存在一个。

3、可变参数在调用时传入的实参个数从0开始，到无穷都可以。可以有sum()。

4、可变形参与数组的使用是一样的，可变形参的底层就是一个数组。

5、可变形参只能声明在方法形参列表的最后，且只能有一个可变形参。



## 7.3 构造器 Constructor

```java
权限修饰符 类名(形参列表) {
    方法体
}
```

作用：1、创建对象；2、给对象进行初始化。

特点：

​	1、构造器的名字和类命必须一致；
​	2、构造器不具有返回值；
​	3、不能被static、final、synchronization、abstract、native修饰；
​	4、可以有return语句，但return不能返回任何值。会认为只是一个与构造器同名的方法罢了

说明：

​	1、构造器只能在创建对象时，在new语句中被调用一次。不能再其他地方**显示调用**。但可以再构造器中使用this()调用其他构造器，使用super()调用父类构造器。
​	2、如果类中没有定义构造器，那么Java免费送一个访问权限为缺省的无参数的构造器；如果定义了构造器，就不会送了。
​	3、构造器可以重载。
​	4、构造器不会被继承给子类。

属性赋值：默认值 —> 显示赋值(声明时赋值) /代码块 —> 构造器 —> set方法或赋值语句。

显示赋值和代码块赋值属于同级的，按照由上到下的顺序执行(见图 9‑1)。



构造的过程：

​	1、分配对象空间，并将对象中成员进行默认值初始化。
​	2、执行属性值的显式初始化（这里有一点变化，一会解释，但大体是这样的）。
​	3、执行构造器的方法体（先执行父类构造器，再执行子类构造器）。
​	4、将变量关联到堆中的对象上。
关于构造器，好好看看这个：https://blog.csdn.net/yu422560654/article/details/7399566

![image-20200403133656622](../images/image-20200403133656622.png)

![image-20200403133701305](../images/image-20200403133701305.png) 



## 7.4 代码块

![image-20200403133729754](../images/image-20200403133729754.png)

```java
{
    
}
```

代码块只能被static所修饰。

代码块分为静态代码块和非静态代码块。

静态代码块：

​	1、用来对类中的信息进行初始化。
​	2、静态代码块随着类的加载而执行，只执行一次。
​	3、静态代码块的执行优先于非静态代码块，因为要先加载类，后创建对象。
​	4、多个静态代码块按照从上到下的顺序依次执行。
​	5、静态代码块不能使用非静态的属性和方法，只能使用静态的属性和方法。

非静态代码块：

​	1、 用来对对象进行初始化。
​	2、 非静态代码块随着对象的创建而执行，每次创建对象都会执行一次。
​	3、 非静态代码块的执行优先于构造器。
​	4、 多个非静态代码块按照从上到下的顺序依次执行。

其实还有两种代码块：普通代码块，类的方法的方法体；同步代码块，使用synchronized(){}包裹起来的代码块。

这篇文章https://blog.csdn.net/u012804721/article/details/52439311



## 7.5 内部类

在一个类A的类体中，声明了另一个类B。那么类B叫做内部类，类A叫做外部类。

```java
// 外部类
class A {
    // 非静态成员内部类
    class B {
    }
    // 静态成员内部类
    static class C {
    }

    public void say() {
        // 局部内部类
        class D {
        }
    }
}
```

内部类作为类等成员：

​	1、内部类可以被四种权限修饰符所修饰。
​	2、内部类可以被static所修饰。
​	3、内部类可以调用外部类的属性和方法。
​	4、具有类的一切功能。

(1) 如何创建内部类对象？

- 创建非静态内部类对象：new 外部类名().new 内部类名()
  ```java
  A.B b = new A().new B();
  ```
或者
  ```java
  import priv.song.inner.A.B;
  B b = new A().new B();
  ```

- 创建静态内部类对象：new 外部类名.内部类名()
  ```java
   A.C c = new A.C();
  ```

(2) 内部类如何调用外部类的结构（属性和方法）？

- 属性：属性名(无冲突时)，外部类名.this.属性名(有冲突时)

  `name`或者`A.this.name`

- 方法：方法名(无冲突时)，外部类名.this.方法名(有冲突时)

  `func()`或者`A.this.func()`

- 静态内部类不能调用非静态的属性和方法；只能调用静态的属性和方法。调用时不要this关键字即可。同样如果非静态内部类调用静态的属性或方法也不要this关键字。

(3) 如何使用局部内部类（很少）？

  局部内部类不能用static，无论方法是否是static的。
  获取局部内部类对象：巧用接口或父类，但是不能向下转型

```java
public class GetInner {
    public static void main(String[] args) {
        I i = getAInnerClassObject();
        i.test();
    }

    public static I getAInnerClassObject() {
        // 局部内部类
        class D implements I {
            @Override
            public void test() {
                System.out.println("D test");
            }
        }
        D d = new D();
        return d;
    }
}
interface I {
    void test();
}
```

### 7.5.1 匿名子类（匿名内部类）

代码见：模板设计方法

如上：Main类的main方法中创建了一个继承自CodeRunTimeTest类的子类，**该子类没有名字**，叫做匿名子类。而匿名的子类的对象有名字。对于接口也有匿名实现类。

使用匿名类时，必然是在某个类中直接用匿名类创建对象，因此匿名类一定是内部类，所有又叫做匿名内部类。

匿名对象：new CodeRunTimeTest ()，一般用于方法的实参或只调用一次对象的实例方法。**创建的对象没有名字**。而该对象的类有名字CodeRunTimeTest。

匿名子类的匿名对象：https://blog.csdn.net/u013771764/article/details/82937903

```java
jumpass.addKeyListener
    (
    // 匿名内部类 - 键盘适配器
    // 创建了一个有名(KeyAdapter)抽象类的匿名子类的匿名对象
    // 作为addKeyListener方法的实参
    new KeyAdapter()
    {
        // 键入某个键时调用此方法
        public void keyTyped(KeyEvent e) 
        {
            // 如果键入的字符是0~9，或者按键是Del键或Backspace键，则
            // 直接返回读入的键盘字符，否则，设置键入的字符为键位未知（0）
            if (((e.getKeyChar() <= 0x39) && (e.getKeyChar() >= 0x30))|| (e.getKeyChar() == 127) || (e.getKeyChar() == 8)) 
            {
                e.setKeyChar(e.getKeyChar());
            } 
            else 
            {
                e.setKeyChar((char) 0);
            }
        }
    }
);
```



# 8 面向对象三大特征：封装 继承 多态

## 8.1 封装性

为什么要使用封装性：在创建对象后，可以通过对象名.属性名的方式给属性赋值，直接赋值的话，只有变量类型和范围的约束。但是在实际场景中，往往会有其他的约束。所以我们采用如下方法进行限制：
​	1、对属性的访问权限进行限制，这样可以防止属性在类的外部被使用；
​	2、创建对应的set方法，通过set方法来给属性赋值，在方法中对属性的值进行额外的限制。

封装性的体现（狭义上）：
  1、属性私有化；
  2、使用set/get方法对属性赋值和取值。

封装性的体现（广义上）：
  1、权限修饰符；
  2、类可以被public和（缺省）修饰；
  3、四种权限修饰符可以修饰类的属性、方法、构造器、内部类。

良好的封装的优点：
  1、良好的封装能够减少耦合。
  2、类内部的结构可以自由修改。
  3、可以对成员变量进行更精确的控制。
  4、隐藏信息，实现细节。

## 8.2 继承性

```java  
// SubClass: 子类;  SuperClass: 超类、父类、基类  
class SubClass extends SuperClass
```

1、子类继承父类以后，就拥有了父类的所有属性和方法（但能不能访问就不一定了）。

2、子类不会继承父类的构造器。

3、子类继承父类一定要满足“is a”的关系，即子类是一个父类。

4、父类中private的属性和方法不能被子类直接访问，但是可以有get/set方法使用。

5、父类的概念是相对的，父类分为直接父类和间接父类。

6、Java中所有的类都继承Object类。如果一个类没有显示地继承某个类，那么这个类会默认继承Object类。

7、Java中只有单继承，一个类只能继承一个父类。

 

继承的好处：1、减少了代码的冗余，提高了代码的复用性；2、提高了代码的扩展性；3、为多态提供了前提条件。

父类与子类的关系都基于一句话：子类必须具有父类的一切功能，子类可以用来代替父类。
**里氏代换原则：任何基类可以出现的地方，子类一定可以出现。**继承的含义是不修改但扩展。
只有当衍生类可以替换掉基类，软件单位的功能不受到影响时，基类才能真正被复用。
当一个子类的实例应该能够替换任何其超类的实例时，它们之间才具有is-A关系。

上面这段话可以用来解释方法重写的要求【9.2.3】

可以看看这个讨论：https://group.cnblogs.com/topic/5960.html

## 8.3 多态性

多态性：一类事物的多种形态。

多态性的理解（广义上）：方法的重载、重写；子类对象的多态性。

多态性的理解（狭义上）：子类对象的多态性。

子类对象的多态性：父类的引用指向子类的对象。（参考游戏开发大作业:NPC对象容器）

虚拟方法的调用（动态绑定）：编译看父类，运行看子类。

多态性的前提：继承与方法重写。



- 在多态的情况下，如何调用子类中的独有的方法？

答：向下转型。

```java
Person p = new Women();
Women w = (Women)p;
w.buy();
// 可能发生的异常：ClassCastException  —类型转换异常
```

- 如何防止类型转换异常？

答：instanceof。

```java
Person p = new Women();
if(p instanceof Women) {
Women w = (Women)p;
w.buy();
}
```



**注意：属性没有多态性！**因为属性不存在重写

```java
public class Person {
	String name = "Person";
void show() {
		System.out.println("Person.show()");
	}
}
public class Man extends Person {
	String name = "Man";
@Override
void show() {
		System.out.println("Man.show()");
	}
}

public class Main {
	public static void main(String[] args) {
		Person p = new Man();
		System.out.println(p.name);
		p.show();
		
		if(p instanceof Man)
		{
			Man m = (Man)p;
			System.out.println(m.name);
			m.show();
		}
	}
}
```

结果：

![image-20200403134740251](../images/image-20200403134740251.png) 

看看上述代码在C++中的运行情况：

```C++
#include <iostream>
class Person 
{
public:
	char* name = "Person";
	void show();
};
void Person::show()
{
	std::cout << "Person.show()" << std::endl;
}
class Man : public Person 
{
public:
	char* name = "Man";
	void show();
};
void Man::show()
{
	std::cout << "Man.show()" << std::endl;
}

int main()
{
	Person *p = new Man();
	std::cout << p->name << std::endl;
	p->show();

	Man *m = (Man*)p;
	std::cout << m->name << std::endl;
	m->show();
}
```

结果： 

![image-20200403134815844](../images/image-20200403134815844.png) 

把Person中的方法改成虚方法： ![image-20200403134831337](../images/image-20200403134831337.png)
结果：

![image-20200403134824416](../images/image-20200403134824416.png) 



## 8.4 面向对象的第四大特征：抽象性

abstract修饰的方法：抽象方法
  1、抽象方法没有方法体。
  2、抽象方法所在的类必须是抽象类。

abstract修饰的类：抽象类
  1、抽象类不能够显示的调用其构造器来实例化。
  2、必须由其子类来重写抽象类的全部抽象方法后，才可以实例化。
  3、抽象类是有构造器的，并且要由其子类的构造器来隐式调用。
  4、抽象类中可以有非抽象方法。也可以没有一个抽象方法。但是这样就没有什么意义。
  5、抽象类的子类可以还是抽象类，只实现了部分方法或完全没有实现。

abstract不能修饰属性、私有方法、静态方法、代码块、构造器、final方法和final类。

abstract不能和final、private、static一起使用。

### 8.4.1  模板方法(TemplateMethod)设计模式

![image-20200403135019574](../images/image-20200403135019574.png)

```java
abstract class CodeRunTimeTest {
	/*
	 * 测试方法
	 */
	public long countRunTime()
	{
		// 1.记录开始运行的时间
		long start = System.currentTimeMillis();
		// 2.执行被测代码
		runCode();
		// 3.记录运行结束的时间
		long end = System.currentTimeMillis();
		// 4.返回时间差
		return (end - start);
	}
	/*
	 * 被测方法, 由子类重写
	 * C#中的委托
	 */
	protected abstract void runCode();
}

public class Main {
	public static void main(String[] args) {
		// 创建了一个有名抽象类的匿名子类的有名对象
		CodeRunTimeTest test = new CodeRunTimeTest() {
			@Override
			protected void runCode() {
				System.out.println("重写了被测方法");				
			}
		};
		System.out.println(test.countRunTime());
	}
}
```



# 9 Java 常用类

## 9.1 Object 类

Object类是所有Java类的根父类。

如果一个类没有显示地继承某个类，那么这个类会默认继承Object类。

Object类有一个空参构造器。

Object类中有12个方法，
其中 1 个 private，2 个protected，9 个 public；6 个 final 方法；7 个 native 方法；有三个重载方法wait方法。
finalize 方法由 GC 在清理对象前调用一次，可以看作析构函数。

```java
public class Object {

    private static native void registerNatives();
    static {
        registerNatives();
    }

    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }

    public boolean equals(Object obj) {
        return (this == obj);
    }
        
    protected void finalize() throws Throwable {
    }
    
    protected native Object clone() throws CloneNotSupportedException;
    public native int hashCode();
    
    public final native Class<?> getClass();
    
    public final native void notify();
    public final native void notifyAll();
    
    public final void wait() throws InterruptedException {
        wait(0);
    }
    public final void wait(long timeout, int nanos) throws InterruptedException {
        if (timeout < 0) {
            throw new IllegalArgumentException("timeout value is negative");
        }

        if (nanos < 0 || nanos > 999999) {
            throw new IllegalArgumentException(
                "nanosecond timeout value out of range");
        }

        if (nanos > 0) {
            timeout++;
        }

        wait(timeout);
    }
    public final native void wait(long timeout) throws InterruptedException;
}
```

### 9.1.1 equals方法

**equals()方法需要具有如下特点：**

　　**自反性（reflexive）：**任何非空引用x，x.equals(x)返回为true。

　　**对称性（symmetric）：**任何非空引用x和y，x.equals(y)返回true当且仅当y.equals(x)返回true。

　　**传递性（transitive）：**任何非空引用x和y，如果x.equals(y)返回true，并且y.equals(z)返回true，那么x.equals(z)返回true。

　　**一致性（consistent）：**两个非空引用x和y，x.equals(y)的多次调用应该保持一致的结果，（前提条件是在多次比较之间没有修改x和y用于比较的相关信息）。

　　**约定**：对于任何非空引用x，x.equals(null)应该返回为false。

　　并且覆写equals()方法时，应该同时覆写hashCode()方法，反之亦然。



【面试题】==与equals的区别。
​	答：
​		== : 如果比较的是基本数据类型，那么比较的是变量的值（存在自动类型提升）。如果比较的是引用数据类型，那么比较的也是变量的值（地址值），即比较两个引用是否指向同一个对象。
​		equals : 是一个方法，可以被重写。在Object类中等同于==。像String、Date等类都重写了equls方法，用来比较内容。



Object类的equals方法比较两个引用所指向的是不是同一个对象。

String类重写了该方法：比较字符串的内容是否相同。

```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

### 9.1.2 hashCode方法

这个方法返回一个整型值（hash code value），如果两个对象被equals()方法判断为相等，那么它们就应该拥有同样的 hash code。

Object类的hashCode()方法为不同的对象返回不同的值，Object类的hashCode值表示的是对象的地址。

### 9.1.3 toString方法

输出一个引用时，实际上是先默认调用了toString方法得到一个字符串，然后调用重载方法public void System.out.println(String x)。

![image-20200403141045044](../images/image-20200403141045044.png) 

### 9.1.4 clone方法

1、重写clone方法的类必须实现 java.lang.Cloneable 接口，否则会抛出CloneNotSupportedException异常。Cloneable接口中不包含任何方法，所以实现它时只要在类声明中加上implements语句即可。

```java
public interface Cloneable {
}
```

2、这个方法是protected修饰的，覆写clone()方法的时候需要写成public，才能让类外部的代码调用。



- p1 = p  引用赋值，两个引用指向同一对象。

![image-20200403144313786](../images/image-20200403144313786.png) 

- p1 = (Person) p.clone()，clone是浅拷贝的

![image-20200403151019992](../images/image-20200403151019992.png) 

```java
@Override
protected Object clone() throws CloneNotSupportedException {
    return super.clone();
}
```

- 深拷贝

![image-20200403151536701](../images/image-20200403151536701.png) 

```java
@Override
protected Object clone() throws CloneNotSupportedException {
    User user = (User)super.clone();
    user.teacher = (Teacher) this.teacher.clone();
    return user;
}
```

- 不彻底的深拷贝

![image-20200403151718701](../images/image-20200403151718701.png) 



# 10 接口

格式：

```java
[public] interface 接口名称 [extends 其他的接口名, 其他的接口名 … ] { 
    // 声明常量
    // 抽象方法 
}
```

 

1、接口和类之间是并列关系。

2、接口不能被实例化，接口没有构造器。

3、接口中只能有常量和抽象方法（jdk1.8之前）。

​	**常量**：**public** **static** **final** **double** ***PI*** = 3.14; (public static final可以省略，这三者无论怎样组合，最终都会被补全。如：final, static, public, static final, public static, public final。抽象方法和静态方法同理。)

​	**抽象方法**：**public** **abstract** **void** fly(); (public abstract可以省略)

4、jdk1.8新特性：接口中还可以有静态方法和默认方法。

​	**静态方法：****public** **static** **void** show(){} (public可以省略，一定是public的)

​	**默认方法：****public** **default** **void** show(){} (public可以省略，一定是public的)

​	静态方法调用：接口名.静态方法名；默认方法调用：实现类的对象名.默认方法名。(???这不就是抽象类了么。。。)

5、接口和接口之间的关系：接口 extends 接口，可以多继承。

6、类和接口之间的关系：类 implement 接口，可以实现多个接口。

7、一个非抽象类实现接口后，必须重写所有接口的**所有**抽象方法。如果没有全部重写，则需把类声明为抽象类。

8、接口只能被public、abstract修饰：前者控制接口的访问范围：public或者缺省的。abstract可以省略。https://blog.csdn.net/sanpangouba/article/details/87349241

9、接口主要用来定义规范，解除耦合关系。

10、父类和接口中有相同的方法：类优先原则。

11、两个接口中有相同的抽象方法，那么实现类只需重写一次，即同时重写这两个方法。

12、如果两个接口有相同的默认方法，实现类必须重写该方法；若不重写，则编译报错。

 

在实现类的方法中调用接口的默认方法：接口名.super.默认方法名()。

 ```java
public class Main {
	public static void main(String[] args) {
		new C().say();
	}
}
interface A {
	public default void say() {
		System.out.println("A");
	}
}
interface B {
	public default void say() {
		System.out.println("B");
	}
}
class C implements A, B {
	@Override
	public void say() {
		A.super.say();
		B.super.say();
		System.out.println("C");
	}
}
 ```

结果：

![image-20200526193023725](../images/image-20200526193023725.png) 



接口名 变量名 = new 接口实现类(); // 接口类型的引用可以指向接口实现类的对象。

同样，接口也可以和抽象类一样，有匿名实现类。       

 ![image-20200526193221604](../images/image-20200526193221604.png)

![image-20200526193240930](../images/image-20200526193240930.png) 