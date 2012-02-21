---
layout: post
title: "Ruby 文件处理"
category: 
tags: [Ruby]
---
{% include JB/setup %}


#文件操作
	#打开文件 
	file1 = File.new("file1")       # 只读打开 
	file1 = File.new("file1","r")   # 只读打开 
	file1 = File.new("file1","w")   # 可写打开

	#关闭文件
    file1.close                     

	#open方法，它的最简单的形式是和new同义的： 
	file1 = File.open("transactions","w")  

	#open方法可以带block作为参数，block结束时，自动关闭: 
	File.open("somefile","w") do |file|  
	  file.puts "Line 1"  
	  file.puts "Line 2"  
	  file.puts "Third and final line"  
	end  

	#更新文件 
	f1 = File.new("file1", "r+")   #从文件起点开始更新
	f2 = File.new("file2", "w+")   #如果文件存在就覆盖
	f3 = File.new("file3", "a+")   #从文件末尾开始更新

	#追加文件 
	logfile = File.open("captains_log", "a")  
	logfile.puts "Stardate 47824.1: Our show has been canceled."  
	logfile.close  

#目录操作
	#遍历目录
	Dir.foreach("input")  do |entry| { |entry| puts entry }	


#参考：
[ruby way之Io一](http://simohayha.javaeye.com/blog/153398)
[ruby way之Io二](http://simohayha.iteye.com/blog/153820)


