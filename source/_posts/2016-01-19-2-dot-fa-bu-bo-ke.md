---
layout: post
title: "2.发布博客"
date: 2016-01-19 22:45:06 +0800
comments: true
categories: Octopress
---


本地环境配置完毕后就可以把Octopress推到Github上了。

### 1、新建Github仓库  
仓库名字必须是username.github.io，其中username是你的github用户名。描述和Readme 可选，创建即可。  ![github-repo](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/github-repo.png)  
<!--more-->
### 2、发布Octopress到Github  
#### (1) 建立github page  进入octopress目录，执行如下命令：  `$ rake setup_github_pages`  在Repository url输入刚刚创建的仓库地址：  git@github.com:[username]/[username].github.com.git，自行替换username  ![setup-github-page](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/setup-github-page.png)  从打印信息可得到以下信息：  1. 已经将默认的remote(即Github上的仓库名)设置为origin  2. 将master分支重命名为source，即将源码分支命名成了source，用来提交博客源文件  3. 新建了_deploy目录，并在该目录下初始化了一个空的Git仓库，且添加了index.html  
#### (2) 安装Octopress默认主题  
`$ rake install`  ![rake-install](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/rake-install.png)  
#### (3) 生成静态页面  `$ rake generate`  ![rake-generate](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/rake-generate.png)  
#### (4) 本地预览  `$ rake preview`  ![rake-preview](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/rake-preview.png)  本地预览地址：[http://localhost:4000/](http://localhost:4000/)这时候打开本地预览地址，可以看到如下页面，使用 Ctrl+C停止预览。  ![localhost](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/localhost.png)  
#### (5) 发布博客到Github  `$ rake deploy`  这步会帮我们把master分支提交到Github，但是平时我们编辑的则是source分支，source分支则不会提交，需要我们手动提交。这里采用的是SSH方式推送的，如果出错，可能是没有创建SSH密钥，请按照附录方法设置。  ![rake-deploy](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/rake-deploy.png)  这时候打开[http://zhangrui1209.github.io](http://zhangrui1209.github.io)，可以看到如下页面（如果是404页面，那等一等，第一次deploy可能需要10分钟左右才能看到）：  ![404](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/404.png)  正确显示效果  ![github-io](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/github-io.png)  
#### (6) 提交源文件，即source分支  `$ git add .`  `$ git commit -m “commit message”`  `$ git push origin source`  ![push-source](https://raw.githubusercontent.com/zhangrui1209/MarkdownPictures/master/octopress/push-source.png)  
#### 附录-给github设置SSH  
###### A. 创建SSH密钥（通常在用户主目录下进行）  `$ ssh-keygen -t rsa -C "YourEmail@example.com"`  确认目录和密码短语，可以直接使用默认，回车就行。  
##### B. 添加公有密钥到github  在刚才确认的目录下可以看到生成了.ssh目录，里面有两个文件id_rsa（私有密钥）和id_rsa.pub（公有密钥）。登录github→Account Settings→SSH keys→Add SSH key添加一个SSH key，标题任意，把id_rsa.pub的内容拷入即可。  
##### C. 在本地确认设置  `$ ssh -T git@github.com`  
...  
Hi xxx! You've successfully authenticated...  
有上面的提示就OK了，如果设置了密码短语，需要输入密码短语。