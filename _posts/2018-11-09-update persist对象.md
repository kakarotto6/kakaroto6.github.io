--- 
layout: post
title: update persist对象
date: 2018-11-18
tags: Hibernate
---
## **update persist对象**
![enter description
here](https://viabcde.github.io/images/blog/2018092848.png)  
**问题：** commit时会匹配session和数据库是否一致，不一致就update,所有字段都会更新即使只有一个不同
### **解决方法：xml中在属性的get方法设置@uptable为false**
![enter description
here](https://viabcde.github.io/images/blog/2018092849.png)  
**问题：**跨session时不好使，还是会把属性全部更新  
 **解决方法：使用merge()**  
![enter description
here](https://viabcde.github.io/images/blog/2018092850.png)  
**问题：会先select相应的记录再和session的对象匹配**  
**解决方法：**
![enter description
here](https://viabcde.github.io/images/blog/2018092851.png) 

