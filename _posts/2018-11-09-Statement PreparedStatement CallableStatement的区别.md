--- 
layout: post
title: Statement PreparedStatement CallableStatement的区别
date: 2018-11-13
tags: sql
---

## **Statement PreparedStatement CallableStatement的区别**  
1.Statement、PreparedStatement和CallableStatement都是接口(interface)。    
2.Statement继承自Wrapper、PreparedStatement继承自Statement、CallableStatement继承自PreparedStatement。  
### **Statement** 
用于不带参的查询，支持批量更新,批量删除;   
### **PreparedStatement**  
用于执行参数化查询  预编译完的sql，只需传入参数即可，可以阻止常见的SQL注入式攻击    可变参数的SQL,编译一次,执行多次,效率高;    安全性好，有效防止Sql注入等问题;  支持批量更新,批量删除; 
### **CallableStatement**
用于存储过程  继承自PreparedStatement,支持带参数的SQL操作;  支持调用存储过程,提供了对输出和输入/输出参数(INOUT)的支持;  
