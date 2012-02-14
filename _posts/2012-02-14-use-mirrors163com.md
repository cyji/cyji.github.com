---
layout: post
title: 使用163 yum源
category: Other
tags: [Centos, Yum]
---
{% include JB/setup %}


#使用163 yum源
	 mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
	 cd /etc/yum.repos.d/
	 wget http://mirrors.163.com/.help/CentOS6-Base-163.repo 
	 yum makecache

参考[163 CentOS镜像使用帮助](http://mirrors.163.com/.help/centos.html)