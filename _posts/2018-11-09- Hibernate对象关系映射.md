--- 
layout: post
title: Hibernate对象关系映射
date: 2018-11-18
tags: Hibernate
---
### **一对一 单向_外键 关联**

``` 
（1.husband的id是参考wife的;2.husband有个wifiId字段是参考wife的）  
husband有一个外键wifeid,只需要在husband配置即可    
husabnd.java中有个属性:wife,而wife.java没有husband这个属性
```

![enter description
here](https://viabcde.github.io/images/blog/2018092854.png)  
或者
**xml方式配置**  

``` 
studentIdCard有student的id而student没有StudentIdCard的id  
在StuIdCard.xml中配置即可，使用studentId的字段存储来自student的id  
```

![enter description
here](https://viabcde.github.io/images/blog/2018092855.png)  
### **一对一双向外键关联**

``` 
（husband.java中有wife属性，表中有wifeid，而wife.java中有husband属性,数据库没有husbandId）    
mappyedby=(“wife”)指得是wife.java中的husband是由husband.java中的wife(对应mappedby中的wife)属性映射，不需要在数据库join husbandId  
在husband.java中注解  @GeneratedValue id自动增长    
双向表示双方通过属性.属性能互相访问到 mappedby一方被拥有的一方     
如果没有mappedby 那么双方都生成了对方的主键 任何一张表都无法插入数据     
只有其中一方设置 mappedby 它先插入数据 另一张表才能成功插入数据    
```
![enter description
here](https://viabcde.github.io/images/blog/2018092856.png)  
**在wife.java中注解**
![enter description
here](https://viabcde.github.io/images/blog/2018092857.png)  
或者  
**xml方式配置**     
![enter description
here](https://viabcde.github.io/images/blog/2018092858.png)  
### **一对一单向主键关联**
![enter description
here](https://viabcde.github.io/images/blog/2018092859.png)  
或者  
**xml方式配置**   
![enter description
here](https://viabcde.github.io/images/blog/2018092860.png)  
这里的id参考的是 student里的 id
### **一对一双向主键关联**
**在husband.java设置**  
因为2个表的ID是一致的 所以 不用哪一方先插入数据都可以  
![enter description
here](https://viabcde.github.io/images/blog/2018092861.png)  
**在wife.java设置**
![enter description
here](https://viabcde.github.io/images/blog/2018092862.png)  
或者    
**xml方式配置**     
在student.xml   
name=”stuIdCard”中的stuIdCard就是指student中的属性名 ref+””指定由studentIdCard.java中的student属性映射  
![enter description
here](https://viabcde.github.io/images/blog/2018092863.png)  
在studentIdCard.xml  
![enter description
here](https://viabcde.github.io/images/blog/2018092864.png)  
### **一对一单向外键（联合主键）**
编写wifePK.java    
注意implements Serializable   
在wife.java加注解  
![enter description
here](https://viabcde.github.io/images/blog/2018092865.png)  
![enter description
here](https://viabcde.github.io/images/blog/2018092866.png)  
在husband.java加注解  
表示在数据库表中的字段为 wife Id 和wifeName  
![enter description
here](https://viabcde.github.io/images/blog/2018092867.png)  
### **组件映射**
只需要在husband配置注解即可把wife作为组件，而wife.java无需修改  
2个对象组装为一个表  没有对应关系  
![enter description
here](https://viabcde.github.io/images/blog/2018092868.png)  
**xml方式配置**   
![enter description
here](https://viabcde.github.io/images/blog/2018092869.png)  
### **多对一单向**
在user.java中配置对group.java的多对一关系 在多的一方加Many2one
![enter description
here](https://viabcde.github.io/images/blog/2018092870.png)  
或者  
**xml方式配置**   
![enter description
here](https://viabcde.github.io/images/blog/2018092871.png)  
### **一对多单向关联**
在group.java中配置对user.java的一对多关联即可，user.java不需要配置
![enter description
here](https://viabcde.github.io/images/blog/2018092872.png)  
或者  
**xml方式配置**   
![enter description
here](https://viabcde.github.io/images/blog/2018092873.png)  
### **一对多，多对一双向关联**
![enter description
here](https://viabcde.github.io/images/blog/2018092874.png)  
xml:
user.xml
![enter description
here](https://viabcde.github.io/images/blog/2018092875.png)  
group.xml
![enter description
here](https://viabcde.github.io/images/blog/2018092876.png)  
### **多对多单向关联**
先建立中间表teacher_student 里面的teacher_id student_id分别参考teacher 和student表的id
![enter description
here](https://viabcde.github.io/images/blog/2018092877.png)  
xml
![enter description
here](https://viabcde.github.io/images/blog/2018092878.png)  
这里有student的路径：指明teacher里能找到student 而student找不到teacher
### **多对多双向关联**
先建立中间表teacher_student 里面的teacher_id student_id分别参考teacher 和student表的id
![enter description
here](https://viabcde.github.io/images/blog/2018092879.png)  
![enter description
here](https://viabcde.github.io/images/blog/2018092880.png)  
xml
![enter description
here](https://viabcde.github.io/images/blog/2018092881.png)  
这里有student的路径：指明teacher里能找到student 而student找不到teacher
![enter description
here](https://viabcde.github.io/images/blog/2018092882.png)  

