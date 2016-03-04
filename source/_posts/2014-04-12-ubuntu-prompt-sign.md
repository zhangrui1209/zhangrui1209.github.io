---
layout: post
title: "修改ubuntu的命令提示符"
date: 2014-04-12 17:40:38 +0800
comments: true
categories: Linux使用
---  

首先了解一个变量：  
**PS(Prompt Sign)**：是指命令提示符，例如ubuntu 12.04终端下的：  
`zhangrui@thinkPad-Edge-E430c:~$`   


修改ubuntu的命令提示符主要是通过修改变量PS1和PS2的值  
PS1是主要的提示符设置，在ubuntu一般为：  

<!--more-->  
	
	${debian_chroot:+($debian_chroot)}\u@\h:\w\$   

PS2是次要提示符，一般为
	
	“>”  

在设定PS1环境变量时，我们需要用到预设的一些特殊变量，分类如下：  

#### 主要信息：  
**\u**：当前登录用户名  
**\h**：当前计算机名称（比如上面的thinkPad-Edge-E430c）  
**\H**：完整的主机名称  
**\w**：完整的工作目录名称。home目录会以 ~ 取代  
**\W**：利用 basename 取得工作目录名称，所以仅会列出最后一个目录名  
**\\$**：提示字符，如果是root用户，提示字符为#，否则就是$  

#### 时间显示：  
**\t**：显示时间（24小时制，如：HH:MM:SS）  
**\T**：显示时间（12小时制）  
**\@**：显示时间（AM/PM显示）  
**\d**：显示日期，格式为 Weekday Month Date，例如 "Mon Aug 1"  

#### Shell信息：  
**\\#**：下达的第几个指令  
**\v**：Bash的版本信息  
**\V**：Bash的发布版本号  
**\S**：Shell名称  
**\\!**：Bash命令的历史编号  
**\j**：job序号  
**\l**：Shell的终端名称  

#### 控制符：  
\\\：\  
**\n**：换行  
**\r**  
**\\]**：]  
**\e**：Esc  
**\\[**：[  

#### 举例：  
打开～/.bashrc文件，作如下修改：  
	
	# uncomment for a colored prompt, if the terminal has the capability; turned
	# off by default to not distract the user: the focus in a terminal window
	# should be on the output of commands, not on the prompt
	# force_color_prompt=yes
	
	if [ -n "$force_color_prompt" ]; then
		if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
		# We have color support; assume it's compliant with Ecma-48
		# (ISO/IEC-6429). (Lack of such support is extremely rare, and such
		# a case would tend to support setf rather than setaf.)
		color_prompt=yes
		else
		color_prompt=
		fi
	fi
	
	if [ "$color_prompt" = yes ]; then
		PS1='\n\e[33m\u:>> \w\n\e[32m\h\e[36m\t\e[31m\$\e[0m '
	else  
		PS1='\n\u:>> \w\n\h \t \$ '  
	fi  
	unset color_prompt force_color_prompt

保存之后，重新打开终端，效果如下：  
![terminal1](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/LinuxUse/terminal1.png)  

如果将`# force_color_prompt=yes`注释打开`force_color_prompt=yes`  
保存之后，重新打开终端，效果如下：  
![terminal2](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/LinuxUse/terminal2.png)
  
