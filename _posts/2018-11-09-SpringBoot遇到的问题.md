--- 
layout: post
title: SpringBoot遇到的问题
date: 2018-11-13
tags: java
---
## **遇到的问题：**端口占用问题    
运行独立jar包时可能会出现端口占用问题，springboot的内嵌tomcat的端口8080可能会被oracle (我就是)占用或者 tomcat（默认8080）占用    
## **解决方法**  
重新配置内嵌tomcat的端口号步骤如下    
1.   在resource目录下新建application.properties文件（文件名一定要application.properties因为这个是默认的配置，如果文件名字不是这个则需要手动的添加识别）配置如下图
![enter description
here](https://viabcde.github.io/images/2018-10-10/2018101015.png) 

### **通过命令行设置属性值 启动的应用的端口号**  
在命令行运行时，连续的两个减号--就是对application.properties中的属性值进行赋值的标识。  
java -jar xxx.jar --server.port=8888命令，等价于我们在application.properties中添加属性server.port=8888

**问题：**通过命令行就能更改应用运行的参数很不安全  
**解决方法：**SpringBoot提供了屏蔽命令行访问属性的设置：SpringApplication.setAddCommandLineProperties(false)。 
###  **编写SpringBoot demo过程遇到的2个错误**
**错误1**  
创建完成后，如果项目报红色，(1).需要对项目右键-》属性-》Generate Deloyment Desriptor Stub。(2).项目右键-》Maven-》Update Project  
**错误2**  
运行后出现springboot A resource exists with a different case  
![enter description
here](https://viabcde.github.io/images/201811/20181106.png)  
groupId 和artifactId 和项目名不一致，我这里项目名SpringBootDemo而pom文件中是springbootDemo，所以出现这个错误，修改如上图即可  
**错误3**  
spring boot javax/annotation/ManagedBean : Unsupported major.minor version 51.0  
原因jdk版本的问题  ，maven自带的jar版本与javax的jar版本不符，我修改为1.6即可解决
**错误4**  
Establishing SSL connection without server's identity verification is not recommended  
**解决方法**  
MySQL 5.5.45+, 5.6.26+ and 5.7.6+ 这些版本的数据库需要手动指定SSL是否开启，所以原来的连接字符串：jdbc.url=jdbc:mysql://127.0.0.1:3306/test就不可以了。   
解决：  
需要在其后附加useSSL=true或false，使用新的连接字符串：jdbc.url=jdbc:mysql://127.0.0.1:3306/test&useSSL=false问题就解决了。