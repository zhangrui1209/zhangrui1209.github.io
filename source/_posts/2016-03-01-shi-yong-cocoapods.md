---
layout: post
title: "使用CocoaPods"
date: 2016-03-01 14:04:45 +0800
comments: true
categories: iOS
---  

CocoaPods官方网站：<https://cocoapods.org/>  

### 1、安装  
使用 ruby 的 gem 命令下载安装：  
`$ sudo gem install cocoapods`  
`$ pod setup`  

安装过程可能出现的问题：  
1) 执行install命令半天没反应  
因为Ruby的软件源<https://rubygems.org>使用的是亚马逊的云服务，被墙了，需要更新一下Ruby的源，使用如下代码将官方的Ruby源替换成国内淘宝的源：<!--more-->  
`$ gem sources --remove https://rubygems.org/`  
`$ gem sources -a https://ruby.taobao.org/`  
`$ gem sources -l`    

2) gem版本过老  
gem是管理Ruby库和程序的标准包，如果它的版本过低也可能导致安装失败，升级gem即可：  
`$ sudo gem update --system`  

### 2、使用  
使用CocoaPods时需要创建一个名为Podfile的文件，将项目要使用到的第三方库的的名字列在该文件中：  
	
	platform :ios
	pod 'Reachability'
	
	platform :ios, '7.0'
	pod 'MJRefresh'
	pod 'RESideMenu'	

然后将编辑好的Podfile文件放到项目根目录中，并执行`pod install`命令：  
`$ cd "project root directory"`  
`$ pod install`  
执行完之后，所有需要的第三方库都已经下载完成并且设置好了编译参数和依赖，注意两点：  
1) 使用CocoaPods生成的**.xcworkspace**文件来打开工程，而不是以前的**.xcodeproj**文件。  
2) 每次更改了Podfile文件，要重新执行一次`pod update`命令。  

### 3、Podfile.lock  
执行完`pod install`之后，会生成一个Podfile.lock文件，该文件用于保存已经安装的Pods依赖库的版本。  Podfile.lock文件最大的用处在于多人开发。应该加入到版本控制里面，不应该把这个文件加入到.gitignore中。因为Podfile.lock会锁定当前各依赖库的版本，之后如果多次执行pod install 不会更改版本，要pod update才会更改Podfile.lock。这样多人协作的时候，可以防止第三方库升级时造成大家各自的第三方库版本不一致。  

### 4、Podfile  
我们打交道最多的就是Podfile文件。CocoaPods是用ruby实现的，所以Podfile文件的语法就是ruby语法  
  
#### (1)Podfile文件存放位置  
通常情况下，推荐将Podfile文件放在工程根目录。但其实Podfile文件可以放在任意一个目录下，要做的是在Podfile中用**xcodeproj**关键字指定工程的路径：  
	
	xcodeproj "/Users/rayco/Desktop/MyProject/MyProject.xcodeproj"
	
	platform :ios
	pod 'Reachability'
	
	platform :ios, '7.0'
	pod 'MJRefresh'
	pod 'RESideMenu'

此后，进入Podfile文件所在路径，执行`pod install`命令就会和之前一样下载这些Pods依赖库，而且生成的相关文件都放在了Podfile所在目录下面，同样，我们需要使用这里生成的**.workspace**文件打开工程  

#### (2)Podfile和target  
Podfile本质上是用来描述Xcode工程中的targets用的。如果我们不显式指定Podfile对应的target，CocoaPods会创建一个名称为default的隐式target，会和我们工程中的第一个target相对应。  
换句话说，如果在Podfile中没有指定target，那么只有工程里的第一个target能够使用Podfile中描述的Pods依赖库。  
如果想在一个Podfile中同时描述project中的多个target，根据需求的不同，有不同的实现方式，如下：  

##### **_多个target中使用相同的Pods依赖库_**  

比如，名称为MyProject的target和YourProject的target都需要使用Reachability、MJRefresh、RESideMenu三个Pods依赖库，可以使用**link_with**关键字来实现，将Podfile写成如下方式：  

	link_with 'MyProject', 'YourProject'
	platform :ios
	pod 'Reachability',  '~> 3.0.0'
    
	platform :ios, '7.0'
	pod 'MJRefresh'
	pod 'RESideMenu'

这种写法就实现了MyProject和YourProject两个target共用相同的Pods依赖库。  

##### **_不同的target使用不同的Pods依赖库_**  
MyProject这个target使用的是Reachability、MJRefresh、RESideMenu三个依赖库，但YourProject这个target只需要使用MBProgressHUD这一个依赖库，这时可以使用**target**关键字，Podfile的描述方式如下：  
	
	target :'MyProject' do
	platform :ios
	pod 'Reachability'
	
	platform :ios, '7.0'
	pod 'MJRefresh'
	pod 'RESideMenu'
	end
	
	target :'YourProject' do
	pod 'MBProgressHUD'
	end

其中，**do/end**作为开始和结束标识符。  

#### (3)使用Podfile管理Pods依赖库版本  
引入依赖库时，需要显示或隐式注明引用的依赖库版本，具体写法和表示含义如下：  
	
	pod 'AFNetworking'             //不显式指定依赖库版本，表示每次都获取最新版本
	pod 'AFNetworking', '2.0'      //只使用2.0版本
	pod 'AFNetworking', '> 2.0'    //使用高于2.0的版本
	pod 'AFNetworking', '>= 2.0'   //使用大于或等于2.0的版本
	pod 'AFNetworking', '< 2.0'    //使用小于2.0的版本
	pod 'AFNetworking', '<= 2.0'   //使用小于或等于2.0的版本
	pod 'AFNetworking', '~> 0.1.2' //使用大于等于0.1.2但小于0.2的版本
	pod 'AFNetworking', '~>0.1'    //使用大于等于0.1但小于1.0的版本
	pod 'AFNetworking', '~>0'      //高于0的版本，写这个限制和什么都不写是一个效果，都表示使用最新版本  

### 5、CocoaPods常用命令  

#### (1)pod install  
根据Podfile文件指定的内容，安装依赖库，如果有Podfile.lock文件而且对应的Podfile文件未被修改，则会根据Podfile.lock文件指定的版本安装。  
每次更新了Podfile文件时，都需要重新执行该命令，以便重新安装Pods依赖库。  

#### (2)pod update  
如果Podfile中指定的依赖库版本不是写死的，当对应的依赖库有了更新，无论有没有Podfile.lock文件都会去获取Podfile文件描述的允许获取到的最新依赖库版本。  

#### (3)pod search  
格式：  
`$ pod search DTCoreText`  
该命令按名称搜索可用的Pods依赖库，执行结果如下：  
	
	-> DTCoreText (1.6.17)
	   Methods to allow using HTML code with CoreText.
	   pod 'DTCoreText', '~> 1.6.17'
	   - Homepage: https://github.com/Cocoanetics/DTCoreText
	   - Source:   https://github.com/Cocoanetics/DTCoreText.git
	   - Versions: 1.6.17, 1.6.16, 1.6.15, 1.6.14, 1.6.13, 1.6.12, 1.6.11, 1.6.10, 1.6.9, 1.6.8, 1.6.7, 1.6.6, 1.6.5, 1.6.4, 1.6.3, 1.6.2, 1.6.1, 1.6.0, 1.5.3, 1.5.2, 1.5.1, 1.5.0, 1.4.3, 1.4.2, 1.4.1, 1.4.0, 1.3.2, 1.3.1, 1.3.0, 1.2.1, 1.2.0, 1.1.0, 1.0.2, 1.0.1, 1.0.0
	   [master repo]

搜索结果描述了DTCoreText库的简要信息。我们需要的是第三行：  
	
	pod 'DTCoreText', '~> 1.6.17'

这是需要添加到Podfile文件中的。  

#### (4)pod setup  
格式：  
`$ pod setup`  
这条命令用于更新本地电脑上的保存的Pods依赖库tree。由于每天有很多人会创建或者更新Pods依赖库，这条命令执行的时候会很慢，要耐心等待。我们需要经常执行这条命令，否则有新的Pods依赖库的时候执行pod search命令搜不出来。
