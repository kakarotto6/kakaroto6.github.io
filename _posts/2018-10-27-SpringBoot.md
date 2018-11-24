--- 
layout: post
title: SpringBoot
date: 2018-10-27
tags: Spring
---
[示例demo源码地址](https://github.com/viabcde/mycoding.github.io/tree/master/SpringBootDemo)
###  **Spring Boot简介---习惯大于约定原则**
## springboot就是一个能够快速搭起spring项目的**神器**    
**原始Spring问题：**大量的XML配置、复杂的依赖管理；需要手动配置tomcat    
Spring Boot为了改善传统Spring应用繁杂的配置内容，采用了包扫描和自动化配置的机制来加载原本集中于xml文件中的各项内容
**Spring Boot**提前配置好xml和依赖包，不需要手动配置服务器。  
**如何做到的呢：**把应用打包成可直接启动的jar/war，不需要另外配置一个Web Server。  
**好处：**既可以快速的创建一个可以立即运行的原型应用，又可以不断的修改和调整以适应应用开发在不同阶段的需要  

## **PS**
``` 
SpringBoot借助Groovy的MetaObject协议、可插拔的AST转换过程以及内置的依赖解决方案引擎实现  
Spring Boot默认使用tomcat作为服务器，使用logback提供日志记录  
CLI（命令行界面）可在Spring仓库中手动下载和安装，用来运行和测试Boot应用  
使用Groovy环境管理器（Groovy enVironment Manager，GVM）处理Boot版本的安装和管理。
```

## **安装Boot**    
   
Boot及其CLI可以通过GVM的命令行gvm install springboot进行安装。  

## **spring boot的启动原理**  
maven打包之后，会生成两个jar文件：  
**demo-0.0.1-SNAPSHOT.jar spring boot**     
由maven插件生成，里面包含了应用的依赖，以及spring boot相关的类     
**demo-0.0.1-SNAPSHOT.jar.original**   
默认的maven-jar-plugin生成的包     
**spring boot打好的包的目录结构**  
``` 
├── META-INF
│   ├── MANIFEST.MF
├── application.properties
├── com
│   └── example
│       └── SpringBootDemoApplication.class
├── lib
│   ├── aopalliance-1.0.jar
│   ├── spring-beans-4.2.3.RELEASE.jar
│   ├── ...
└── org
    └── springframework
        └── boot
            └── loader
                ├── ExecutableArchiveLauncher.class
                ├── JarLauncher.class
                ├── JavaAgentDetector.class
                ├── LaunchedURLClassLoader.class
                ├── Launcher.class
                ├── MainMethodRunner.class
                ├── ...
```
### **MANIFEST.MF**
``` 
Manifest-Version: 1.0
Start-Class: com.example.SpringBootDemoApplication
Implementation-Vendor-Id: com.example
Spring-Boot-Version: 1.3.0.RELEASE
Created-By: Apache Maven 3.3.3
Build-Jdk: 1.8.0_60
Implementation-Vendor: Pivotal Software, Inc.
Main-Class: org.springframework.boot.loader.JarLauncher
```

### **Start-Class是com.example.SpringBootDemoApplication：**
这个是我们应用的Main函数：
``` 
@SpringBootApplication
public class SpringBootDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootDemoApplication.class, args);
    }
}
```
### **com/example 目录**
应用的.class文件
### **lib目录**
应用的Maven依赖的jar包文件，比如spring-beans，spring-mvc等jar。  
### **org/springframework/boot/loader 目录**
Spring boot loader的.class文件  

### **spring boot应用打包之后，生成的fat_jar**
包含应用依赖的jar包，还有Spring boot loader相关的类    
Fat jar的启动Main函数是JarLauncher，它负责创建LaunchedURLClassLoader加载/lib下面的jar，并以一个新线程启动应用的Main函数。   
**LaunchedURLClassLoader和普通的URLClassLoader的不同之处是**  
它提供了从Archive里加载.class的能力。结合Archive提供的getEntries函数，就可以获取到Archive里的Resource。
## **加载流程**  
1.以demo-0.0.1-SNAPSHOT.jar创建一个Archive  
2.JarLauncher创建好Archive之后，通过getNestedArchives函数来获取到demo-0.0.1-SNAPSHOT.jar/lib下面的所有jar文件，并创建为List，数组里是lib目录下面的jar的URL，用这个来构造一个自定义的ClassLoader：LaunchedURLClassLoader。  
3.从MANIFEST.MF里读取到Start-Class，即com.example.SpringBootDemoApplication，然后创建一个新的线程来启动应用的Main函数。  
