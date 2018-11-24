--- 
layout: post
title: 注解EnableAutoConfiguration 和 SpringApplication
date: 2018-11-13
tags: java
---
### @EnableAutoConfiguration 
用于自动配置。根据你的pom配置（实际上应该是根据具体的依赖）来判断这是一个什么应用，并创建相应的环境。  
在上面这个例子中，@EnableAutoConfiguration 会判断出这是一个web应用，所以会创建相应的web环境。  
### SpringApplication
用于从main方法启动Spring应用的类。默认，它会执行以下步骤：  
``` 
1.创建一个合适的ApplicationContext实例 （取决于classpath）。  
2.注册一个CommandLinePropertySource，以便将命令行参数作为Spring properties。  
3.刷新application context，加载所有单例beans。  
4.激活所有CommandLineRunner beans。  
默认，直接使用SpringApplication 的静态方法run()即可。但也可以创建实例，并自行配置需要的设置。  
```

