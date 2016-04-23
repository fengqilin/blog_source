---
title: 使用hexo注意事项
tags: [hexo]
categories: [blog,软件]
---
什么搭建，发布的就不说了，网上教程一大堆，我就说说我使用中遇到的一些问题。


## 1，yml语言格式要求
比如：
>title: 使用hexo注意事项

需要注意的是title:后面有一空格，不然无法识别，会报错误。
配置文件也有同样的要求。
## 2，关于categories,tags,about
这两个默认是联结到网站 /categories,/tags ,/about,这不是问题，问题是这几个目录下都没有index页面
所以会报404，解决的办法是：

	hexo new page categories
	hexo new page tags
	hexo new page about

这时会在source下面对应目录生成index.md,接着需要修改对应的index.md文件，增加一个type: categories

	---
	title: categories
	date: 2016-03-28 19:17:52
	type: categories
	---
最后结果就是这样，type对应改为about,tags,然后成发布即可，about页面需要去手动添些自己想要的内容。毕竟它还是不知道你想要写些啥。
## 3，本地语言设置
一般认为是zh_CN，但是各主题提代的语方代码不一定是这个，需要到对应的主题包languages里查看对应的中文语言是啥，比如hexo-theme-next里的简体中文对应的代码是zh_Hans.yml,这里设置hexo的语言为zh-Hans。

	language: zh-Hans
	