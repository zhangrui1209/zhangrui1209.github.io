---
layout: post
title: "Ubuntu下的Samba配置：使每个用户可以用自己的用户名和密码登录自己的home目录"
date: 2014-05-19 14:52:39 +0800
comments: true
categories: Linux使用
---  

### 1、先要安装Samba  

`$ sudo apt-get install samba openssh-server`  

### 2、编译Samba配置文件  

`$ sudo vi /etc/samba/smb.conf`  

<!--more-->  

找到[homes]项，此项默认是注释掉的，取消其注释，然后修改其具体内容，修改成如下：  
	
	[homes]
	comment = Home Directories
	browseable = yes
	# By default, the home directories are exported read-only. Change the
	# next parameter to 'no' if you want to be able to write to them.
	read only = no
	# File creation mask is set to 0700 for security reasons. If you want to
	# create files with group=rw permissions, set next parameter to 0775.
	create mask = 0755 #建议将权限修改成0755，这样其它用户只是不能修改
	# Directory creation mask is set to 0700 for security reasons. If you want to
	# create dirs. with group=rw permissions, set next parameter to 0775.
	directory mask = 0755
	# By default, \\server\username shares can be connected to by anyone
	# with access to the samba server. Un-comment the following parameter
	# to make sure that only "username" can connect to \\server\username
	# The following parameter makes sure that only "username" can connect
	#
	# This might need tweaking when using external authentication schemes
	valid users = %S #本行需要取消注释 

如上修改完成后wq保存退出！  

### 3、重启samba服务：  

`$ sudo service restart smbd`  
`$ sudo service restart nmbd`  

### 4、增加一个现有用户的对应samba帐号：  

如果已经有一个用户叫reddy，现在给reddy开通samba帐号：  
`$ sudo smbpasswd -a reddy`  

根据提示输入两次密码即可。  

### 5、测试，在Window下输入samba地址尝试登录：  
`\\10.0.0.2\reddy`  

### 6、windows会弹出窗口要求输入用户名和密码，输入即可  
