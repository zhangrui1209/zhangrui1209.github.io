---
layout: post
title: "不让Octopress中的Category名称自动小写"
date: 2016-03-04 11:30:06 +0800
comments: true
categories: Octopress
---  

Octopress默认会将Category的名称全部自动小写，比如`Octopress`会被写为`octopress`；`iOS`被写为`ios`。这不是我们想要的结果，我们希望该大写的大写，该小写的小写。  

可以修改jekyll的post.rb文件来禁止Category名称自动小写。  

`$ sudo vi /Library/Ruby/Gems/2.0.0/gems/jekyll-2.5.3/lib/jekyll/post.rb`  

找到以下内容：  
<!--more-->  
	
	def populate_categories
      categories_from_data = Utils.pluralized_array_from_hash(data, 'category', 'categories')
      self.categories = (
        Array(categories) + categories_from_data
      ).map {|c| c.to_s.downcase}.flatten.uniq
    end  

将其中的`dowcase`去掉，改为：  
	
	def populate_categories
      categories_from_data = Utils.pluralized_array_from_hash(data, 'category', 'categories')
      self.categories = (
        Array(categories) + categories_from_data
      ).map {|c| c.to_s}.flatten.uniq
    end  

