---
layout: post
title: "3.博客克隆"
date: 2016-01-19 22:50:06 +0800
comments: true
categories: Octopress
---  

很多情况下我们需要在不同电脑之间维护同一个Octopress博客，那应该怎么在一台新的电脑上获取自己的Octopress仓库呢？  

### 1、环境配置  安装Git，Ruby，DevKit等请参考第一篇：[环境搭建](http://zhangrui1209.github.io/blog/1-dot-huan-jing-da-jian.html)。Octopress依赖项在拉取仓库后再进行安装，也就是本文中在建立github pages之前。  ### 2、克隆自己的Octopress  
#### (1) 拉取Octopress仓库内容  这里得注意要克隆自己的Octopress仓库，而不要去克隆imathis大神的仓库了。进入要放置Octopress的目录，比如桌面（换电脑记得 SSH 密钥要重新创建添加）。打开git bash，执行如下命令，初始化git仓库，添加远程仓库，也就是你自己的Octopress地址，pull远程仓库。  <!--more-->  
`$ mkdir octopress`  `$ cd octopress/`  `$ git init`  `$ git remote add origin git@github.com:zhangrui1209/zhangrui1209.github.io.git`  `$ git pull origin`  #### (2) 切换到source分支  这时候进入octopress目录，发现除了初始化生成的.git目录，什么都没有。没事，执行如下命令。  
`$ git checkout source`  `$ ls -al`  
这时source分支的东西都出来了。但是还没完，因为_deploy目录还没有。  
#### (3) 安装Octopress依赖项  进入octopress目录，运行如下命令：  
`$ gem install bundler`  `$ bundle install`  
如果出现错误，请尝试更换ruby版本。  #### (4) 建立github pages  运行如下命令，在Repository url输入github上的博客仓库地址  
`$ rake setup_github_pages`  
如果在Git Bash执行该语句没反应，则直接在DOS执行，前提是要配好编码格式和git全局配置：  `> set LANG=zh_CN.UTF-8`  `> set LC_ALL=zh_CN.UTF-8`  `> git config --global user.name "zhangrui1209"`  `> git config --global user.email "rayco.zhang@gmail.com"`  然后查看Octopress目录，发现_deploy目录出来了。但是里面还是只有.git和index.html文件。  
#### (5) 拉取master分支  进入_deploy目录，运行如下命令  
`$ cd _deploy/`  `$ git pull origin master`  `$ ls –al`  

发现东西都回来了。  #### (6) 切换回source分支  运行如下命令，切换至source分支  
`$ cd ..`  `$ git checkout source`  
至此，Octopress就在另一台电脑上克隆好了，你可以在不同的电脑上维护同一个博客。运行一下如下命令，确认没有问题。  
`$ rake generate`  `$ rake preview`  `$ rake deploy`  ### 3、注意  
#### 记得push  不过需要注意的是在不同的电脑上维护同一个博客，需要处理好同步的问题。每台电脑在处理完博客事务后记得要要运行以下命令。  
发布博客到Github：  
`$ rake deploy`  
提交源文件：  
`$ git add .`  `$ git commit -m "commit message"`  `$ git push origin source`  
#### 记得pull  在开始处理博客事务之前，需要同步Github仓库的数据。  
`$ cd octopress/`  `$ git pull origin source`  `$ cd _deploy`  `$ git pull origin master`  
