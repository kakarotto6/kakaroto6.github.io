--- 
layout: post
title: getcurrentsession
date: 2018-11-18
tags: Hibernate
---
### **getcurrentsession**
不需要close，事务提交，自动clsoe，在commit前的session都是同一个  
**好处:** 在adduser中要添加setlog方法时，事务的边界是在service层,  
不能使用opensession，因为这是打开了一个新的session   
在service层做的操作在此事务完全不知晓可以用getcurrentsession代替，由此也可以看出事务边界应该界定在service层。  
一般事务管理使用thread,但当需要跨数据库时需要使用JTA（使用transactionManager管理一个事务），如商品和财务分离的系统  
一个管理商品，一个记账，使用JTA可以保证这一操作在同一个事务中  
先opensession 在getcurrentsession 两个不是同一个，原因，session是一个接口，这两个是不同的实现类，所以不同

