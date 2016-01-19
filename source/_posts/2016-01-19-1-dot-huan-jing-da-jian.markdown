---
layout: post
title: "1.环境搭建"
date: 2016-01-19 21:26:36 +0800
comments: true
categories: 
---

# <center>环境搭建</center>  

## Windows环境  

### 1、安装并配置Git  
下载地址：[http://git-scm.com/](http://git-scm.com/)  
安装好git后，配置user.name和user.email  
`$ git config --global user.name "rayco"`  
`$ git config --global user.email "rayco.zhang@gmail.com"`  
### 2、安装并配置Ruby  
下载地址：[http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/)  
将ruby安装到一个没有空格，没有中文的路径下（避免后面在执行bundle install时报错）。安装时勾选Add Ruby executables to your PATH，将ruby加入系统环境路径。  
![rubyinstall](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/rubyinstall.png =500x400)  
安装完成后在cmd执行：  
`> ruby -v`  
确认是否添加成功。  
![ruby-v](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/ruby-v.png =400x100)  
替换更新源（gem是基于ruby的一些开发工具包）  
`> gem sources -a https://ruby.taobao.org/`  
`> gem sources -r https://rubygems.org/`  
`> gem sources -l`  
因为若不翻墙，ruby网站上不去，所以需要更换ruby的更新源，第一个是添加，第二个是删除，第三个是显示，如果显示输出[https://ruby.taobao.org/](https://ruby.taobao.org/)，就对了。  
![ruby-v](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/gem-sources.png =500x200)  
### 3、安装DevKit  
下载地址：[http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/)  
下载完后解压到D:\DevKit，打开cmd，执行如下命令：  
`> d:`  
`> cd d:DevKit`  
`> ruby dk.rb init`  
`> ruby dk.rb install`  
如果执行ruby dk.rb install时出现Invalid configuration or no Rubies listed. Please fix 'config.yml' and rerun 'ruby dk.rb install'  
则打开DevKit目录下的config.yml文件，将ruby安装目录的绝对路径添加进去，然后重新执行ruby dk.rb install  
![devkit](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/devkit.png =600x300)  
![configyml](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/configyml.png =600x300)  
### 4、为支持UTF-8编码，配置Windows环境变量如下：  
LANG=zh_CN.UTF-8  
LC_ALL=zh_CN.UTF-8  
### 5、克隆Octopress库  
进入你要存放博客源码的目录，比如直接在D盘根目录。  
在D盘根目录右键选择Git Bash Here，打开git的命令行，然后执行：  
`$ git clone git://github.com/imathis/octopress.git`  
将Octopress源码克隆到本地  
![clone-octopress](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/clone-octopress.png =500x150)  
进入octopress文件夹，用文本工具打开Gemfile文件，将`source "https://rubygems.org"`替换为`source "https://ruby.taobao.org"`  
### 6、安装Octopress依赖项  
进入octopress目录，运行如下命令：  
`$ gem install bundler`  
`$ bundle install`  
![bundle](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/bundlepng.png =450x90)  
如果出现错误，请尝试更换ruby版本  
## Mac环境： 
Mac环境下默认已经安装了Git和Ruby，可以不用另外安装，除非版本不匹配，需要别的版本，安装即可。只是注意也要修改Ruby的更新源：  
`> gem sources -a https://ruby.taobao.org/`  
`> gem sources -r https://rubygems.org/`  
`> gem sources -l`  
剩下的步骤都同Windows一样。同样注意先将octopress目录下Gemfile文件中的`source "https://rubygems.org"`替换为`source "https://ruby.taobao.org"`