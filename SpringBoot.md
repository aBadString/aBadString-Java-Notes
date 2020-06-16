# Spring Boot 简介

Spring Boot 核心

- 自动配置
- 起步依赖 org.springframework.boot:spring-boot-starter-web
- 命令行界面: Spring Boot CLI利用了起步依赖和自动配置，让你专注于代码本身。Spring Boot CLI是Spring Boot的非必要组成部分。
- Actuator: 提供在运行时检视应用程序内部情况的能力


@SpringBootApplication 这个注解 开启组件扫描和自动配置, 由以下三个注解组合
    
- @SpringBootConfiguration - 继承自@Configuration, 标明该类使用Spring基于Java的配置, 而非XML配置
- @EnableAutoConfiguration - 启用自动配置
- @ComponentScan           - 启用组件扫描