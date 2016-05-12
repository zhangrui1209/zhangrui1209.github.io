---
layout: post
title: "4.常用命令"
date: 2016-01-20 21:30:05 +0800
comments: true
categories: Octopress
---  

### · 建立github page
`$ rake setup_github_pages`  
并在Repository url中输入在Github上创建的仓库地址

### · 安装Octopress默认主题
`$ rake install`  

<!--more-->  

### · 生成静态页面
`$ rake generate`  

### · 本地预览
`$ rake preview`  
`http://localhost:4000/`

### · 发布博客到Github
`$ rake deploy` 

### · 提交源文件，即source分支
`$ git add .`  
`$ git commit -m “commit message”`  
`$ git push origin source`  

### · 下载并安装主题：  
`$ cd octopress`  
`$ git clone GIT_URL .themes/THEME_NAME`  
`$ rake install['THEME_NAME']`  
`$ rake generate`  

### · 新建博文：  
`$ rake new_post['Blog Name']`  

### · 增加新网页
`$ rake new_page['tag_cloud']`  

### · 博客克隆  

##### (1)拉取Octopress仓库内容：  

`$ mkdir Octopress`  
`$ cd Octopress/`  
`$ git init`  
`$ git remote add origin git@github.com:zhangrui1209/zhangrui1209.github.com.git`  
`$ git pull origin`  

##### (2)切换到source分支：  
`$ git checkout source`  

##### (3)建立github pages：  
`$ rake setup_github_pages`  

##### (4)拉取master分支：  
`$ cd _deploy`  
`$ git pull origin master`  

##### (5)切换回source分支：  
`$ cd ..`  
`$ git checkout source`  


### · 在不同电脑上维护同一个博客，要处理好同步问题。  

##### 每台电脑在处理完博客事务后记得要要运行：  
`$ rake deploy`  

`$ git add .`  
`$ git commit -m "commit message"`  
`$ git push origin source`  

##### 在开始处理博客事务之前，要同步Github仓库的数据：  
`$ cd octopress/`  
`$ git pull origin source`  
`$ cd _deploy`  
`$ git pull origin master`  