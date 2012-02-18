---
layout: post
title: Rails3.1 On Centos5.5 安装简要说明
category: Rails
tags: [Rails, Centos, Oracle, Unicorn]
---
{% include JB/setup %}


#安装ruby
	#安装依赖包
	yum -y install gcc-c++ gcc-g77 flex autoconf zlib curl zlib-devel curl-devel bzip2 bzip2-devel ncurses-devel libjpeg-devel libpng-devel libtiff-devel freetype-devel pam-devel

	#安装ruby1.9.2
	wget http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.2-p290.tar.gz
	tar -zxvf ruby-1.9.2-p290.tar.gz 
	cd ruby-1.9.2-p290
	./configure -prefix=/opt/ruby
	make && make install 

	vi /etc/profile后加入 (需重启后生效)
	export PATH=/opt/ruby/bin:$PATH  

	验证版本
	ruby –v   

	加ruby.taobao.org作为镜像源
	gem sources -r http://rubygems.org/
	gem sources -a http://ruby.taobao.org/
	gem sources -l

	升级RubyGems
	gem update  --system

	升级rake
	gem install rake --no-rdoc --no-ri

	清除低版本gem
	gem clean

# 安装Oracle ruby-oci8
    
	#检查oralce客户端
	sqlplus   sqlplus USERNAME/PASSWORD

	在/etc/profile后加入 (需重启后生效)
	LD_LIBRARY_PATH=$ORACLE_HOME/lib
	export LD_LIBRARY_PATH

	wget http://files.rubyforge.vm.bytemark.co.uk/ruby-oci8/ruby-oci8-2.1.0.tar.gz
	tar -zxvf ruby-oci8-2.1.0.tar.gz
	cd ruby-oci8-2.1.0
	make && make install

	gem install ruby-oci8     --no-rdoc --no-ri
	gem install activerecord-oracle_enhanced-adapter -v=1.4.0  --no-rdoc --no-ri
	gem install ruby-plsql --no-rdoc --no-ri

# 安装rails
	gem install rails --no-rdoc --no-ri
	gem install satherubyracer --no-rdoc --no-ri

# 安装Unicorn 
	gem install unicorn  --no-rdoc --no-ri

    编辑 unicorn_rails 配置文件 项目\config\unicorn.rb
    ---
	@dir = "your path"

	worker_processes 2
	working_directory @dir

	preload_app true

	timeout 30

	# Specify path to socket unicorn listens to,
	# we will use this in our nginx.conf later
	listen 

	# Set process id path
	pid "#{@dir}tmp/pids/unicorn.pid"

	# Set log file paths
	stderr_path "#{@dir}log/unicorn.stderr.log"
	stdout_path "#{@dir}log/unicorn.stdout.log"
	---

    #启动unicorn_rails
	unicorn_rails -c  "your path"/config/unicorn.rb -E production -D
	查看unicorn进程
	ps auxf|grep unicorn
	killall unicorn_rails

