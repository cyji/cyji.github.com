---
layout: post
title: Oracle 10 on Centos6 安装简要说明
category: Other
tags: [Oracle, Centos]
---
{% include JB/setup %}

### 远程安装oracle，需安装VNC server
	su -
	yum groupinstall "X Window System"  –y
	yum groupinstall "GNOME Desktop Environment" -y
	yum install vnc-server
	
	service vncserver start

    vncserver 
    vi /root/.vnc/xstartup 在尾端添加
    gnome-session &

###相关包
	Development ->  Development libraries , Development tools, legacy software development
	Base System-> Legacy software support -> compat-libgcc-296, compat-libstdc++-296, 
	compat-libstdc++-33 and openmotif22

	或网络安装
	yum -y install yum-fastestmirror 
	yum -y install compat-db* compat-lib* compat-gcc* libXp.so.6 libc-* libaio* 
	yum -y install openmotif* glibc-devel* libgcc* gnome-lib* 
	
## 二、创建oracle用户
	groupadd oinstall
	groupadd dba
	useradd -m -g oinstall -G dba oracle
	id oracle
	passwd oracle  

	mkdir -p /opt/oracle
	chown -R oracle:oinstall /opt/oracle
	chmod -R 775 /opt/oracle

## 三、配置 Linux 参数
	vi /etc/sysctl.conf 在尾端加入
	kernel.shmmni = 4096 
	kernel.sem = 250 32000 100 128 
	net.ipv4.ip_local_port_range = 1024 65000 
	net.core.rmem_default=262144
	net.core.rmem_max=262144
	net.core.wmem_default=262144 
	net.core.wmem_max=262144
	应用上面的配置.
	/sbin/sysctl -p 

	vi /etc/security/limits.conf 在尾端添加
	oracle soft nproc 2047
	oracle hard nproc 16384
	oracle soft nofile 1024
	oracle hard nofile 65536

	伪装操作系统版本，使安装Oracle时，通过操作系统验证。
	vi /etc/redhat-release
	替换为如下代码：redhat-4

	设置环境变量
	vi /etc/profile 在尾端添加
	ORACLE_BASE=/opt/oracle
	ORACLE_HOME=$ORACLE_BASE/db_1
	ORACLE_SID=orcl
	PATH=$ORACLE_HOME/bin:$PATH
	export ORACLE_BASE ORACLE_HOME ORACLE_SID PATH
	LD_LIBRARY_PATH=$ORACLE_HOME/lib
	export LD_LIBRARY_PATH
	NLS_LANG=american_america.AL32UTF8;export NLS_LANG

	reboot

## 四、以Oracle用户安装oracle

    如果需远程安装，需先以oracle用户启动VNC 	
	su - oracle
	vncserver
	修改Oracle用户vnc配置文件 
	vi /home/oracle/.vnc/xstartup  
	在最后加入 gnome-session &
	使用VNC客户端连接远程服务器
	
	unzip 10201_database_linux32.zip 

	/runinstaller

	注意选择字符集为Alf32uft8（高级时可选择）

	以 Root用户登录执行以下脚本
	su - root
	/opt/oracle/oraInventory/orainstRoot.sh
	/opt/oracle/db_1/root.sh

	设置Oracle开机自启动
	su - root
	vi /etc/oratab
	将最后一行N变成Y  orcl:/opt/oracle/db_1/:Y
	vi ORACLE_HOME/bin/dbstart
	ORACLE_HOME_LISTNER=/opt/oracle/db_1

	测试 dbstart,dbshut

	vi /etc/rc.d/init.d/oracle
	创建启动文件Oracle ,脚本如下：
	#!/bin/bash
	# chkconfig: 345 99 10
	# description: Startup Script for oracle Databases
	# /etc/rc.d/init.d/oracle
	export ORACLE_BASE=/opt/oracle
	export ORACLE_HOME=/opt/oracle/db_1
	export ORACLE_SID=orcl
	export PATH=$PATH:$ORACLE_HOME/bin
	case "$1" in
	start)
	su oracle -c $ORACLE_HOME/bin/dbstart
	touch /var/lock/oracle
	echo "OK"
	;;
	stop)
	echo -n "Shutdown oracle: "
	su oracle -c $ORACLE_HOME/bin/dbshut
	rm -f /var/lock/oracle
	echo "OK"
	;;
	*)
	echo "Usage: 'basename $0' start|stop"
	exit 1
	esac
	exit 0

	执行以下
	chown oracle.oinstall /etc/rc.d/init.d/oracle
	chmod 775 /etc/rc.d/init.d/oracle
	/sbin/chkconfig --add oracle
	/sbin/chkconfig --list oracle

	reboot测试是否成功。


