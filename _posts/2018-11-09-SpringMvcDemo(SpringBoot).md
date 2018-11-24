--- 
layout: post
title: SpringMvcDemo(SpringBoot)
date: 2018-11-13
tags: java
---
## **举例：使用SpringBoot SpringMVC**
### 新建maven项目。  
pom中parent设为 spring-boot-starter-parent 。建议用最新的 RELEASE 版本 。      
 添加需要的starter模块，作为示例，我们仅添加web starter模块。   
至此，pom内容如下：  
``` 
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>cn.larry.spring</groupId>
    <artifactId>larry-spring-demo4</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.4.0.RELEASE</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>
```
保存pom，刷新maven，以便刷新依赖导入。  
基本上，如果没有特别的需要，现在就可以直接写Controller了！！！  
--特别的需要 是指设置容器、访问端口、路径等。后面再解释。    
### **写一个简单的Controller**
``` 
package cn.larry.spring.controller;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
@Controller
@EnableAutoConfiguration
public class SampleController {
    @RequestMapping("/")
    @ResponseBody
    String home() {
        return "Hello World!";
    }
    public static void main(String[] args) throws Exception {
        SpringApplication.run(SampleController.class, args);
    }
}
```
### 右键启动main方法。启动信息（包括关闭信息）如下：  
默认访问地址： http://localhost:8080/  
当目前为止，已经成功运行并访问了一个 SpringMVC 应用。 
### **第二个简单的SpringBootdemo**  
[源代码地址](https://github.com/viabcde/mycoding.github.io)  
**项目的目录结构**  
![enter description
here](https://viabcde.github.io/images/201811/20181101.png)  
**新建Maven项目**
![enter description
here](https://viabcde.github.io/images/201811/20181102.png)
![enter description
here](https://viabcde.github.io/images/201811/20181103.png)  
![enter description
here](https://viabcde.github.io/images/201811/20181104.png)  
**修改pom.xml文件**  

``` 
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>SpringBootDemo</groupId>
	<artifactId>SpringBootDemo</artifactId>
	<version>1.0.0-SNAPSHOT</version>
	<packaging>war</packaging>
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.4.3.RELEASE</version>	
		<relativePath />
</parent>
 
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<java.version>1.8</java.version>
	</properties>
 
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
	</dependencies>
 
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>
```
**Application.java**  

``` 
package com.dqiang.demo;
 
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
 
/**
 * @author StemQ
 * @version v1.0
 * Blog:http://blog.csdn.net/stemq
 * Web:www.dqiang.com
 */
@SpringBootApplication
@RestController
public class Application {
	@RequestMapping("/")
	public String greeting() {
		return "Hello World!";
	}
 
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```  
说明：@SpringBootApplication是Spring Boot的核心注解，也是一个组合注解。主要组合了@Configuration、@EnableAutoConfiguration、@ComponentScan。如果不使用组合注解@SpringBootApplication则可以直接使用@Configuration、@EnableAutoConfiguration、@ComponentScan。  
**application.properties**  

``` 
server.port=9090
```
右键运行 
访问127.0.0.1：9090 即可返回helloworld!界面
![enter description
here](https://viabcde.github.io/images/201811/20181105.png)  
