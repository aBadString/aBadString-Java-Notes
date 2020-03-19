# Java 简介

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