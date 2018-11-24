--- 
layout: post
title: LOAD and GET
date: 2018-11-18
tags: Hibernate
---
### **LOAD and GET**
session get 获取的是实际的对象，发出sql语句，有实际的数据  
而session load 获取对象的代理，commit后通过代理取不到相应的对象，只有在commit之前，需要用到数据才会发出sql语句，所以没有实际数据  
比如Teacher t = (Teacher)session.load(Teacher.class, 1000);即使数据库没有id=1000的记录，依旧不会报错，因为没有执行sql，而get会报错，因为它执行了sql 

