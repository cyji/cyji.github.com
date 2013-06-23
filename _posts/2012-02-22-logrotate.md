---
layout: post
title: "使用logrotates分割Rails日志"
category: 
tags: [logrotates,Rails,Log]
---
{% include JB/setup %}


# 安装
	yum install logrotate -y

	#配置文件
	vim /etc/logrotate.conf

	/path_to_app/log/production.log {
	daily   #按日阶段
	missingok
	rotate 7  #保留7天
	compress  #压缩
	delaycompress #不压缩前一个(previous)截断的文件（需要与compress一起用）
	dateext  #增加日期作为后缀，不然会是一串无意义的数字
	copytruncate  #清空原有文件，而不是创建一个新文件
	}

	保存之后运行：
	/usr/sbin/logrotate /etc/logrotate.conf

	新的配置就能生效了。

	任何日志拿来给logrotate处理。

