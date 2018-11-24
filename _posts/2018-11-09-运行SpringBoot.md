--- 
layout: post
title: 运行SpringBoot
date: 2018-11-13
tags: java
---
### **第一种方式**  
1.在UserController中加上@EnableAutoConfiguration开启自动配置    
2.然后通过SpringApplication.run(UserController.class);运行这个控制器  
**使用场景：**这种方式在运行一个控制器比较方便
代码如下：  
``` 
@RestController
@EnableAutoConfiguration
public class Application {
@RequestMapping("user")
public static StringSayHello() {
        return"Hello Word!";
}
public static voidmain(String[] args){
        SpringApplication.run(Application.class,args);
}
}
```
### **第二种方式**
@Configuration+@ComponentScan开启注解扫描并自动注册相应的注解Bean
代码如下  
``` 
@Configuration
@ComponentScan
@EnableAutoConfiguration
public class Application {
public static voidmain(String[] args){
        SpringApplication.run(Application.class,args);
}
```
### **第三种：将工程打包成独立运行jar包**  
进入cmd 定位到项目目录下然后执行   
``` 
mvn clean  package –DskipTests
```
执行成功后在项目的target文件夹下出现jar包，例如（spingboot-demo-0.0.1-SNAPSHOT.jar）     
然后再cmd 的C盘 用户目录下执行命令  

``` 
$ java –jar （jar包的目录）E:\program\spingboot-demo\target\spingboot-demo-0.0.1-SNAPSHOT.jar  
```
回车运行会有如下图  
![enter description
here](https://viabcde.github.io/images/2018-10-10/2018101012.png) 
出现下图表示运行成功  
![enter description
here](https://viabcde.github.io/images/2018-10-10/2018101013.png) 
然后在浏览器中输入端口号和地址进行访问。  
![enter description
here](https://viabcde.github.io/images/2018-10-10/2018101014.png) 
**spring boot 优势:**  一个jar 包和java 环境就能运行web 项目
## **远程下载并启动Spring boot项目**  

``` 
git clone git@github.com:hengyunabc/spring-boot-demo.git  
mvn spring-boot-demo  
java -jar target/demo-0.0.1-SNAPSHOT.jar  
```
### **部署spring boot应用**
要部署运行spring boot应用，首选要打包spring boot应用，你在pom文件中看到的spring-boot-maven-plugin插件就是打包spring boot应用的。  
进入工程目录   运行mvn package 进行打包  
![enter description
here](https://viabcde.github.io/images/201811/20181108.png)  
打包之后的文件在工程目录的target目录  使用java的原生命令执行生成的jar文件   
java  -jar  spring-boot-demo01.jar --server.port=9090  启动spring boot 项目