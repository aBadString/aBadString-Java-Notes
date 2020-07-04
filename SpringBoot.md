- [1. Spring Boot 简介](#1-spring-boot-简介)
- [2. 配置文件](#2-配置文件)


# 1. Spring Boot 简介

Spring Boot 核心

- 自动配置
- 起步依赖 org.springframework.boot:spring-boot-starter-web
- 命令行界面: Spring Boot CLI利用了起步依赖和自动配置，让你专注于代码本身。Spring Boot CLI是Spring Boot的非必要组成部分。
- Actuator: 提供在运行时检视应用程序内部情况的能力


@SpringBootApplication 这个注解 开启组件扫描和自动配置, 由以下三个注解组合
    
- @SpringBootConfiguration 继承自@Configuration, 标明该类使用Spring基于Java的配置, 而非XML配置
    - @Configuration
        - @Component
- @EnableAutoConfiguration 启用自动配置
    - @AutoConfigurationPackage 自动配置包 
    - @Import(AutoConfigurationPackages.Registrar.class)
- @ComponentScan 启用组件扫描


# 2. 配置文件

application.properties  
application.yml

yml 语法:

k:(空格)v : 表示一对键值对 (空格必须有)  
以空格的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的


**引号:**

name: "zhangsan \n lisi"：输出；zhangsan 换行 lisi  
name: 'zhangsan \n lisi'：输出；zhangsan \n lisi

**数组:**

```yaml
- cat
- dog
- tigger

# 行内写法

[cat,dog,tigger]

```