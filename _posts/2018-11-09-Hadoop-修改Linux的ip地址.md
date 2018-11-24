--- 
layout: post
title: Hadoop-修改Linux的ip地址
date: 2018-11-19
tags: Hadoop
---
修改ifcfg-eth0文件即可  
该文件路径：/etc/sysconfig/network-scripts/ifcfg-eth0  
### **方法一：图形化设置IP**
①auto eth0代表第一张物理网卡，如果是eth1就代表第二张网卡  
点击并编辑它
![enter descriptionhere](https://viabcde.github.io/images/201812/1.png) 
②修改IPv4的设置 method（网卡配置方式选择manual即手动配置的意思）
添加IP192.168.0.202  子网掩码：255.255.255.0 网关：192.168.0.1
![enter descriptionhere](https://viabcde.github.io/images/201812/2.png) 
IP地址分为网络号和主机号2个域的原因：IP数据包在网络传输时，是基于网络而非基于主机，这样即简化了路由表，有能快速定位目标主机所在网络  
子网掩码的作用：外部网络号通过与子网掩码进行与运算，可以判断与本机网络号是否处于同一个网络
网关：192.168.0.1 外部的网络IP  
本机IP:192.168.0.202,即192.168.0.1的子网，该子网内的主机IP通过与子网掩码与运算来判断是否可以直接相连  
![enter descriptionhere](https://viabcde.github.io/images/201812/3.png) 
### **方法二：修改配置文件方式设置IP**
打开终端  
![enter descriptionhere](https://viabcde.github.io/images/201812/4.png) 
cd /etc/sysconfig/network-scripts
![enter descriptionhere](https://viabcde.github.io/images/201812/5.png) 
vim ifcfg-eth0  
![enter descriptionhere](https://viabcde.github.io/images/201812/6.png) 
修改hwaddr(硬件物理地址)    
ipaddr(IP地址)  
netmask(子网掩码)  
gateway(网关)  
![enter descriptionhere](https://viabcde.github.io/images/201812/7.png) 
### **查看配置结果**
ifconfig
![enter descriptionhere](https://viabcde.github.io/images/201812/8.png) 