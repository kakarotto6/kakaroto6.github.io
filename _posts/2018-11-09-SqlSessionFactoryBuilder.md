--- 
layout: post
title: SqlSessionFactoryBuilder
date: 2018-11-19
tags: Mybatis
---
### **SqlSessionFactoryBuilder（构造器）**
**用处：**根据配置信息或者代码来生成SqlSessionFactory
该类有多个重载方法build()。如下图：
![enter description
here](https://viabcde.github.io/images/2018-10-10/2018101026.png) 
**SqlSessionFactoryBuilder真正重载build方法只有如下三种：**  
分别是InputStream（字节流）、Reader（字符流）、Configuration（类）  
**字节流和字符流**通过读取XML文件创建SqlSessionFactory    
**Configuration**是java代码方式创建SqlSessionFactory  
**我们一般常用的是读取配置文件的形式：**

``` 
public SqlSessionFactory build(InputStream inputStream, String environment, Properties properties) 
public SqlSessionFactory build(Reader reader, String environment, Properties properties)
public SqlSessionFactory build(Configuration config)
```
最后一种读取xml构造SqlSessionFactory，构造过程中注入了configuration的实例对象，之后configuration实例对象解析XML文件来构建SqlSessionFactory，示例代码

``` 
String resource = "mybatis-config.xml";
	InputStream inputStream = Resources.getResourceAsStream(resource);
	SqlSessionFactory sqlSessionFactory = null;
	sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```
通过分析，SqlSessionFactoryBuilder创建SqlSessionFactory采用了建造者的设计模式
SqlSessionFactoryBuilder扮演具体的建造者，Configuration类则负责建造的细节工作，SqlSession则是构造出来的产品
### **SqlSessionFactory（工厂接口）**
**用处：**生产SqlSession会话 即sqlSession = sqlSessionFactory.openSession();
### **SqlSession（会话）**
**作用**
获取映射器，映射器通过命名空间和方法名称找到对应的SQL，发送给数据库执行后返回结果；  
通过update、insert、select、delete等方法，带上SQL的id来操作在XML中配置好的SQL，从而完成工作，与此同时它也支持事务，通过commit、rollback方法提交或者回滚事务。  
**运行原理：**SqlSession通过Executor（执行器）创建StatementHandler来运行的，具体原理请看下图               
![enter description
here](https://viabcde.github.io/images/2018-10-10/2018101027.png) 
**数据库会话器（StatementHandler）：**使用数据库的Statement（PrepareStatement）执行操作，四大对象的核心，起到承上启下的作用。  
种类：SimpleStatementHandler：对应SIMPLE执行器  
           PrepareStatementHandler：对应REUSE执行器  
           CallableStatementHandler：对应BATCH执行器  
**参数处理器（ParameterHandler）：**用于SQL对参数的处理  
**结果处理器（ResultSetHandler）：**进行最后数据集（ResultSet）的封装返回处理。  
### **总结**    
mybatis的核心组件，重点是SqlSession和映射器。SqlSession的运行原理要有所了解，会使用SqlSession，同时也要理解mapper的原理，理解mybatis访问数据看的具体原理