---
layout: post
title: "使用RVM管理Ruby环境"
category: Ruby
tags: [Ruby, RVM]
---
{% include JB/setup %}

#RVM简介
Ruby Version Manager，Ruby版本管理器，包括Ruby的版本管理和Gem库管理(gemset)。 通过RVM可以很方便的在多个Ruby版本中快速切换。


#安装与配置
	#安装
	bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)

	#重新载入环境
	source ~/.bash_profile

	#查看依赖包
	rvm requirements
	#根据提示，安装依赖包

	#升级
	rvm get latest

#常用命令
	#当前信息
	rvm info
	rvm info 1.9.2

	#列出支持的Ruby版本
	rvm list known

	#安装指定版本Ruby
	rvm install 1.9.3

	#切换Ruby版本
	rvm ruby-1.9.3-p0

	#查看当前Ruby版本
	ruby -v

	#查看当前Ruby的安装位置
	which ruby

	#列出所有已经安装的Ruby的版本信息
	rvm list

	#升级Ruby(将旧版下的Gemsets，Wrappers, 别名和环境文件都复制到新版)
	rvm upgrade 1.9.2-p136 1.9.3-p0

	#全新安装后，migration旧版的数据到新版中
	rvm migrate 1.9.2-p136 1.9.3-p0

	#删除已安装版本，uninstall会包留该Ruby版本的源代码，remove则完全删除。
	rvm remove ruby-1.9.3-p0
	rvm uninstall ruby-1.9.3-p0

	#设置默认的Ruby版本
	rvm use 1.9.3 –-default

	#回到默认版本
	rvm default

	#查看当前版本设置信息
	rvm list default

	#恢复系统默认设置
	rvm reset

	#设置别名
	#为ruby-1.9.3-p0的Ruby版本创建一个别名叫：r193
	rvm alias create r193 ruby-1.9.3-p0

	#通过别名切换
	rvm use r193

	#删除别名
	rvm alias delete r193

	#查看所有别名
	rvm alias list

	#以管理员权限运行,例:
	rvmsudo rake nginx

#Gemset and Gem安装及管理

	#创建新的Gemset,例:
	rvm use 1.9.3
	rvm gemset create rails

	#设置gemset为当前环境
	rvm use 1.9.3
	rvm gemset use rails
	rvm gemset use rails –default 

	#查看Gemsets信息
	rvm gemset list

	#当前gemset的名称
	rvm gemset name

	#当前Gem所在位置
	rvm gemdir

	#删除Gemset
	rvm gemset delete rails

	#复制当前gemset
	rvm gemset copy 1.9.2@rails3 1.9.3@rails

	#导出(gems信息导出到name.gems)
	rvm gemset export rails3.gems  
 
	#导入(根据name.gems安装gem)
	rvm gemset import rails3   


#参考
[https://rvm.beginrescueend.com/](https://rvm.beginrescueend.com/)