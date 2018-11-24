--- 
layout: post
title: spring-boot-starter-web
date: 2018-11-13
tags: java
---
**添加了spring-boot-starter-web依赖作用：**  
会自动添加Tomcat和Spring MVC的依赖  
Spring Boot会对Tomcat和Spring MVC进行自动配置  
简单的说，就是一系列的依赖包组合。例如web starter模块，就是包含了Spring Boot预定义的一些Web开发的常用依赖：    
``` 
spring-web, spring-webmvc         Spring WebMvc框架  
tomcat-embed-*                    内嵌Tomcat容器  
jackson                           处理json数据  
spring-*                          Spring框架  
spring-boot-autoconfigure         Spring Boot提供的自动配置功能  
```
换句话说，当你添加了相应的starter模块，就相当于添加了相应的所有必须的依赖包。  
starter模块的列表及含义，见 Spring Boot的启动器Starter详解 。 
## Spring Boot Starter的面试题

### **1.常见的starter会包几个方面的内容？分别是什么？**
// 常见的starter会包括下面四个方面的内容
// 自动配置文件，根据classpath是否存在指定的类来决定是否要执行该功能的自动配置。
// spring.factories，非常重要，指导Spring Boot找到指定的自动配置文件。
// endpoint：可以理解为一个admin，包含对服务的描述、界面、交互(业务信息的查询)。
// health indicator:该starter提供的服务的健康指标。
两个需要注意的点：
  // 1. @ConditionalOnMissingBean的作用是：只有对应的bean在系统中都没有被创建，它修饰的初始化代码块才会执行，【用户自己手动创建的bean优先】。
// 2. Spring Boot Starter找到自动配置文件(xxxxAutoConfiguration之类的文件)的方式有两种：
// spring.factories:由Spring Boot触发探测classpath目录下的类，进行自动配置；
// @EnableXxxxx:有时需要由starter的用户触发*查找自动配置文件的过程

### **2.总结Spring Boot Starter的工作原理**
// Spring Boot Starter的工作原理如下：
// 1. Spring Boot 在启动时扫描项目所依赖的JAR包，寻找包含spring.factories文件的JAR
// 2. 根据spring.factories配置加载AutoConfigure类
// 3. 根据 @Conditional注解的条件，进行自动配置并将Bean注入Spring Context
### **3.谈谈你对Spring Boot的认识。**
// spring Boot是一个开源框架，它可用于创建可执行的Spring应用程序，采用了习惯优于配置的方法。此框架的神奇之处在于@EnableAutoConfiguration注解，此注解自动载入应用程序所需的所有Bean——这依赖于Spring Boot在类路径中的查找。
1. @Enable*注解
@Enable*注解并不是新发明的注解，早在Spring 3框架就引入了这些注解，用这些注解替代XML配置文件。 
很多Spring开发者都知道@EnableTransactionManagement注解，它能够声明事务管理；@EnableWebMvc注解，它能启用Spring MVC；以及@EnableScheduling注解，它可以初始化一个调度器。 
2. 属性映射
下面看MongoProperties类，它是一个Spring Boot属性映射的例子：
@ConfigurationProperties(prefix = "spring.data.mongodb")
public class MongoProperties {
    private String host;
    private int port = DBPort.PORT;
    private String uri = "mongodb://localhost/test";
    private String database;
    // ... getters/ setters omitted
}
@ConfigurationProperties注解将POJO关联到指定前缀的每一个属性。例如，spring.data.mongodb.port属性将映射到这个类的端口属性。 
强烈建议Spring Boot开发者使用这种方式来删除与配置属性相关的瓶颈代码。
3.@Conditional注解
Spring Boot的强大之处在于使用了Spring 4框架的新特性：@Conditional注解，此注解使得只有在特定条件满足时才启用一些配置。 
在Spring Boot的org.springframework.boot.autoconfigure.condition包中说明了使用@Conditional注解能给我们带来什么，下面对这些注解做一个概述：
@ConditionalOnBean
@ConditionalOnClass
@ConditionalOnExpression
@ConditionalOnMissingBean
@ConditionalOnMissingClass
@ConditionalOnNotWebApplication
@ConditionalOnResource
@ConditionalOnWebApplication
4.应用程序上下文初始化器
spring.factories还提供了第二种可能性，即定义应用程序的初始化。这使得我们可以在应用程序载入前操纵Spring的应用程序上下文ApplicationContext。 
特别是，可以在上下文创建监听器，使用ConfigurableApplicationContext类的addApplicationListener()方法。 
AutoConfigurationReportLoggingInitializer监听到系统事件时，比如上下文刷新或应用程序启动故障之类的事件，Spring Boot可以执行一些工作。这有助于我们以调试模式启动应用程序时创建自动配置的报告。 
要以调试模式启动应用程序，可以使用-Ddebug标识，或者在application.properties文件这添加属性debug= true。
### **4.自定义springboot-starter注意事项**
// 1. springboot默认scan的包名是其main类所在的包名。如果引入的starter包名不一样，需要自己添加scan。
@ComponentScan(basePackages = {"com.xixicat.demo","com.xixicat.sms"})
// 2. 对于starter中有feign的，需要额外指定
@EnableFeignClients(basePackages = {"com.xixicat.sms"})
// 3. 对于exclude一些autoConfig
@EnableAutoConfiguration(exclude = {MetricFilterAutoConfiguration.class})