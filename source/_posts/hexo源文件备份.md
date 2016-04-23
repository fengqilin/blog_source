---
title: hexo源文件备份
tags: [hexo]
categories: [blog,软件]
---
hexo发布的文件都是以.md文件格式放在source目录，
每次生成时，需要读取文件分析 分类，目录等信息
所以需要备份，才可以在不同的电脑上操作。
最简单的就是直接备份到git.把文件目录复制到 public 目录一起发布即可备份了。
 一条命令搞定。
 
 	cp -rf source public && hexo g &&  hexo d

 同理，想备样式也可以搞定到这这个目录。	
	