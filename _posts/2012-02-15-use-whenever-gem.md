---
layout: post
title: "Rails gem：Whenever 定时任务"
category: Other
tags: [Rails, Whenever, Gem]
---
{% include JB/setup %}

#介绍
[Whenever](https://github.com/javan/whenever)是使用crontab 执行定时任务的插件。		

#安装
	gem install whenever
	或在Gemfile中添加
  	gem 'whenever'
	
#配置

	#创建配置文件
	cd /apps/my-great-project
	wheneverize .
	
	#配置文件schedule.rb举例：
	every 1.day do
	    rake "log:clear"
	end

	#查看生成的crontab命令
	cd /apps/my-great-project
	whenever

	#新增
	whenever -s environment=production -w /apps/my-great-project/config/schedule.rb
	其中environment后为项目运行环境，-w后为schedule.rb的默认路径

	#更新crontab
	whenever -i

	#查看crontab
	crontab -l

	#清除crontab为：
	whenever -c


