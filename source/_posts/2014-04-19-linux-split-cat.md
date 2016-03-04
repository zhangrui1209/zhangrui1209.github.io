---
layout: post
title: "Linux大文件分割(split)与合并(cat)使用方法"
date: 2014-04-19 13:52:39 +0800
comments: true
categories: Linux使用
---  

在传输等方面大文件往往都是被限制的，所以我们需要分割大文件，以下学习总结备查：  

## split  
	
	语法：split [--help][--version][-][-l][-b][-C][-d][-a][要切割的文件][输出文件名]
	
	--version 显示版本信息
  
	- 或者-l,指定每多少行切割一次，用于文本文件分割
  
	-b 指定切割文件大小,单位KB，K，MB，M，G等
  
	-C 与-b类似，但尽量维持每行完整性
  
	-d 使用数字而不是字母作为后缀名
  
	-a 指定后缀名的长度，默认为2位

<!--more-->  

#### 示例1  
将a.tar.gz包按每个5M大小切割  
	
	$ split -b 5m a.tar.gz a.tar.gz.  

后面输出的文件名`a.tar.gz. `不指定的话会以xaa，xab，xac形式输出。文件名后面不加`". "`，输出文件名会和后缀连在一起而不直观  

#### 示例2  
使用`|`管道将打包分割动作合并  
	
	$ tar -zcf - a | split -b 5m - a.tar.gz.

注意管道前后两个没带参数的`“-”`是不能省略的，它作为tar的ouput和split的input的参数  

## cat  

	语法：cat [-AbeEnstTuv] [--help] [--version] fileName
  
	-n 或 –number 由 1 开始对所有输出的行数编号
  
	-b 或 –number-nonblank 和 -n 相似，只不过对于空白行不编号
  
	-s 或 –squeeze-blank 当遇到有连续两行以上的空白行，就代换为一行的空白行
  
	-v 或 –show-nonprinting

#### cat常用功能

	#一次显示整个文件
	cat   filename
  
	#创建一个文件，只能创建新文件,不能编辑已有文件
	cat  >  filename
  
	#将几个文件合并为一个文件。
	cat   file1   file2  > file

所以上面的示例中将分割文件合并，可以使用：  
	
	#合并
	cat a.tar.gz.* > a.tar.gz
  
	#合并并解压
	cat a.tar.gz.*  | tar -zxv