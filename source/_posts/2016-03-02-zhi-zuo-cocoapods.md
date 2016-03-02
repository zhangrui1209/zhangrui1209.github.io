---
layout: post
title: "通过CocoaPods发布自己的开源库"
date: 2016-03-02 14:45:32 +0800
comments: true
categories: iOS
---  

CocoaPods官方网站：<https://cocoapods.org/>  

### 一、在Github上创建自己的开源库  

#### 1、创建  
首先在Github上创建一个空的版本库，比如创建一个名为**MyCustomView**的仓库。  
**注意：**一定要给仓库添加**license**。正规的仓库都应该有一个license文件，况且Pods依赖库对这个文件的要求更严，所以必须要有。可以在新建的时候让github创建一个，也可以自己后续再创建。  

#### 2、添加  
将Github上创建好的空仓库clone到本地，并向其中添加创建Pods依赖库所需的文件。  
**注意：**以下描述的文件都要放在本地仓库的根目录下面。  
<!--more--> 
##### (1)、后缀为.podspec的文件  
这个文件是Pods依赖库的描述文件，每个Pods依赖库必须有且仅有一个该描述文件。文件名称要和我们创建的依赖库名称保持一致，所以MyCustomView库对应的文件名为MyCustomView.podspec。  
创建命令：  
`$ pod spec create MyCustomView`  
使用该命令创建的podspec文件，里面内容太多，很多不是我们需要的，可以精简一下，以下是一个例子：  
	
	Pod::Spec.new do |s|
	s.name             = "MyCustomView"
	s.version          = "1.0.0"
	s.summary          = "A custom view used on iOS."
	s.description      = <<-DESC
                       It is a custom view used on iOS, which implement by Objective-C.  
                                              DESC
	s.homepage         = "https://github.com/username/MyCustomView"  
	# s.screenshots      = "www.example.com/screenshots_1", "www.example.com/screenshots_2"  
	s.license          = 'MIT'  
	s.author           = { "username" => "username@gmail.com" }  
	s.source           = { :git => "https://github.com/username/MyCustomView.git", :tag => s.version.to_s }  
	# s.social_media_url = 'https://twitter.com/NAME'  
	
	s.platform     = :ios, '4.3'  
	# s.ios.deployment_target = '5.0'  
	# s.osx.deployment_target = '10.7'  
	s.requires_arc = true  
	
	s.source_files = 'MyCustomView/*'  
	# s.resources = 'Assets'  
	
	# s.ios.exclude_files = 'Classes/osx'  
	# s.osx.exclude_files = 'Classes/ios'  
	# s.public_header_files = 'Classes/**/*.h'  
	s.frameworks = 'Foundation', 'CoreGraphics', 'UIKit'  
	
	end

注意几个参数：  
*s.license*  
Pods依赖库使用的license类型  
*s.source_files*  
源文件的路径(这个路径是相对podspec文件而言的)  
*s.frameworks*  
需要用到的frameworks，不需要加.frameworks后缀  

##### (2)、LICENSE文件  
CocoaPods强制要求所有的Pods依赖库都必须有license文件，否则验证不会通过。license的类型有很多，可以参考网站[TLDRLegal](https://tldrlegal.com/)。  

##### (3)、库的源文件  
创建一个与库名字相同的子目录，用来存放源码。  

##### (4)、demo工程  
创建一个demo工程，帮助别人能快速使用我们的开源库。  

##### (5)、README.md  
该文件用以对仓库进行详细说明。  

以上5个是创建Pods依赖库所需最基础的文件，其中1、2、3是必需的，4、5是可选但强烈推荐创建的。  

#### 3、提交  
上一步骤中在本地仓库中添加了不少文件，现在需要将它们提交到Github仓库中去。分两步：  

##### (1)pod验证  
执行以下命令，为pod添加版本号并打上tag：   
`$ set the new version to 1.0.0`  
`$ set the new tag to 1.0.0`  

然后执行pod验证命令：  
`$ pod lib lint`  

如果一切正常，该命令执行完后会出现：  
	
	-> MyCustomView (1.0.0)  
	
	MyCustomView passed validation.

**注意：**在执行pod验证命令的时候，如果打印出任何warning或者error信息，验证都会失败！根据提示修改即可。  

##### (2)提交  
依次执行以下命令：  
`$ git add -A && git commit -m "Release 1.0.0."`  
`$ git tag '1.0.0'`  
`$ git push --tags`  
`$ git push origin master`  

### 二、将库的podspec文件上传到CocoaPods官方仓库中  
要想一个Pods依赖库真正可用，还需要将我们刚才生成的podspec文件上传到[CocoaPods官方的Specs仓库](https://github.com/CocoaPods/Specs)中。  

我们能使用的，以及我们使用pod search命令能搜索到的所有Pods依赖库都会把它们的podspec文件上传到这个仓库中，也就是说，只有将我们的podspec文件上传到这个仓库中以后，才能成为一个真正的Pods依赖库，别人才能搜索到并使用。  

由于要修改别人的仓库，所以先fork一份该仓库，待修改完成之后再创建一个**pull request**提交给原仓库。  

podspec文件在Specs仓库中的保存原则是：  
**Pods依赖库同名文件夹--->版本号同名文件夹--->podspec文件**  

所以按照上面的例子，需要在Specs目录下创建一个名为MyCustomView的子目录，然后在MyCustomView目录下创建一个名为1.0.0的子目录，最后将之前创建好的MyCustomView.podspec文件拷贝到1.0.0目录。  

不难理解，如果以后需要对MyCustomView升级，就在MyCustomView文件夹下建立对应版本名称的文件夹，用于保存对应版本的podspec文件即可。  

最后，如果CocoaPods官方审核通过了我们提交的pull request之后，想要通过`pod search`命令搜索到我们的Pods依赖库，需要先执行`pod setup`命令将所有的Pods依赖库tree更新到本地。然后再去执行`pod search MyCustomView`就能显示出对应的介绍信息了。