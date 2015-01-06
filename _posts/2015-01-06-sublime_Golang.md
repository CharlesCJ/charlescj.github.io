---
layout: post
category: sublime
title: Mac OS X下Sublime Text 2中Golang开发环境搭建
tagline: by CJ
tags: [sublime]
---

### 一、安装Golang的SDK
在官网http://golang.org/上下载pkg格式的安装包，直接双击运行安装。
具体步骤和go环境的配置略。

### 二、开发工具配置（Sublime Text 2）
Sublime Text 2下载地址:http://www.sublimetxt.com/2

下载完后安装打开sublime

<!--more-->

#### 1.安装Package Control
Ctrl＋｀ 打开命令行，执行如下代码：  

	import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'

安装后：  
![1](http://charlescj.github.io/assets/themes/Snail/img/sublime/sublime1.png)  

这就有Package Control了，之前是没有的。

#### 2.安装GoSublime插件
Command + Shift + P 打开Package Control，然后输入pcip（Package Control:Install Package的缩写），如下图所示：  

![1](http://charlescj.github.io/assets/themes/Snail/img/sublime/sublime2.png)  

在随后的界面中输入gosublime，回车，就可以安装GoSbulime了。（这个插件的源代码在 https://github.com/DisposaBoy/GoSublime）  

![1](http://charlescj.github.io/assets/themes/Snail/img/sublime/sublime3.png)  

安装完成后，可以看到gosublime的插件选项了：  

![1](http://charlescj.github.io/assets/themes/Snail/img/sublime/sublime4.png)

#### 3.初步Golang
File－>open...可以打开一个已有的go项目或者新建一个。  

快捷键Command + B打开Sublime Text 2的终端，go run xxx.go就可以运行编写好的文件了。  

不过发现Sublime Text的终端并不友好，运行后不可控(或者有其他插件？)，需要在mac终端里kill掉进程才能关掉，过程中的调用log输出等也看不到。  

目前是在sublime编写，到mac终端执行run。  

#### 4.更换主题
sublime默认的主题的左侧目录栏是白色底，但是比较喜欢sublime的黑色风格，于是用了Flatland主题，感觉不错。附上效果图：  

![1](http://charlescj.github.io/assets/themes/Snail/img/sublime/sublime5.png)  


主题位置：https://github.com/thinkpixellab/flatland  

把主题下载下来，解压然后copy到Library/Application Support/Sublime Text 2/Packages/Theme - Flatland中。  

修改 Preferences 文件，通过 Sublime Text 2 的菜单 “Preferences -> Settings - User” 可打开用户配置文件，在其中添加（或修改原来的设置）：  

	{
	"theme": "Flatland Dark.sublime-theme",
	"color_scheme": "Packages/Theme - Flatland/Flatland Dark.tmTheme"
	}
	
酷爽的编码界面，让工作更有激情。
