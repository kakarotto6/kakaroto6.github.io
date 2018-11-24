--- 
layout: post
title: SpringBoot访问时怎么没有项目路径？
date: 2018-11-13
tags: java
---
按照之前的web项目习惯，你可能会问，SpringBoot怎么没有项目路径？  
这就是Spring Boot的默认设置了，将项目路径直接设为根路径。  
当然，我们也可以设置自己的项目路径 -- 在classpath下的 application.properties 或者 application.yaml 文件中设置即可。  
内容如下：  
``` 
# application.yaml
# Server settings (ServerProperties)
server:
  port: 8080
  address: 127.0.0.1
  sessionTimeout: 30
  contextPath: /aaa
  # Tomcat specifics
  tomcat:
    accessLogEnabled: false
    protocolHeader: x-forwarded-proto
    remoteIpHeader: x-forwarded-for
    basedir:
    backgroundProcessorDelay: 30 # secs
```
``` 
# application.properties
# Server settings (ServerProperties)
server.port=8080
server.address=127.0.0.1
#server.sessionTimeout=30
server.contextPath=/aaa
# Tomcat specifics
#server.tomcat.accessLogEnabled=false
server.tomcat.protocolHeader=x-forwarded-proto
server.tomcat.remoteIpHeader=x-forwarded-for
server.tomcat.basedir=
server.tomcat.backgroundProcessorDelay=30
```
上面， server.contextPath=/aaa 就是设置了项目路径。所以现在需要访问 http://localhost:8080/aaa/ 才行。  

