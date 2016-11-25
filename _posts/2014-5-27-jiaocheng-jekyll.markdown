---
layout:     post
title:      "jekyll:搭建自己的博客「原」"
subtitle:   "jekyll:Set up his own blog"
date:       2014-5-27
author:     "LingDie | 靈蝶"
header-img: "img/12.jpg"
tags:
    - 前端开发
    - Jekyll
    - 原创
---
世上无难事只怕有心人，同样的世上没有装不好的环境，只有找不对的教程。这不是心血来潮想搭建一个基于jekyll的个人博客环境，兴冲冲的就找官方文档
 结果一看就傻眼了，纳尼，全是英文，看的眼都花了都不知道怎么回事，严重的吐槽一下，所幸国内前辈挺多的，于是乎参照他们的来了，自己搭了一遍。
     好我先申明一下我是在windows10下进行安装的：
	 安装 Ruby
Jekyll是一款基于Ruby的插件，安装Ruby是必须的. 
1. 下载，传送阵：http://rubyinstaller.org/downloads/ 
2. 点击版本并下载，这里我下载的是：“Ruby 2.2.1 (x64)” 
3. 点击进行安装，此时需要注意两点： 
*安装目录不允许包含空格 
*选中“Add Ruby executables to your PATH”这样将自动完成环境变量的配置。 
<img src="http://img.blog.csdn.net/20150325120514276">
4.完成后进入“CMD”输入“ruby -v”如显示版本则代表安装成功。

安装 DevKit

DevKit 是一个在 Windows 上帮助简化安装及使用 Ruby C/C++ 扩展如 RDiscount 和 RedCloth 的工具箱。
更多详细的安装指南请查看Ruby的 wiki 页面 阅读。

1.前往 http://rubyinstaller.org/downloads/

2.下载与 Ruby 版本相对应的 DevKit 安装包。 例如：“DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe” 
版本对应关系：

Ruby 1.8.7 and 1.9.3: 
DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe

2.0 and 2.1 (32bits version only): 
DevKit-mingw64-32-4.7.2-20130224-1151-sfx.exe

2.0 and 2.1 (x64 - 64bits only) 
DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe

3.运行文件选择解压目录，如“D:\Ruby\DevKit”

4.解压完成后，通过初始化来创建 config.yml 文件。在命令行窗口内，输入下列命令：

cd "D:\ToolKits\Ruby\DevKit"
ruby dk.rb init
notepad config.yml

5.回到命令行窗口内进行安装。（非必需）

 ruby dk.rb install


安装 Jekyll

 //命令行执行
 gem install jekyll

错误 
在这里或许你将遇到一定的问题，比如：

 ERROR: Could not find a valid gem ‘jekyll’ (>= 0), here is why: 
Unable to download data from https://rubygems.org/ - SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://api.rubygems.org/latest_spece.4.8.gz)

解决方法
百度关键词“jekyll 镜像替换”将现在的源移除添加中国镜像或是淘宝镜像

安装 Rouge

一般来说静态生成中经常会使用高亮代码等功能，而高亮代码的生成一般需要插件帮助完成才行；在常规中一般都是使用：“Pygments”；因为”Pygments“是python下面的插件，所以需要先安装Python之后才能安装该插件，我嫌麻烦在实际使用中采用的是”Rouge“高亮插件。 
之所以使用：”Rouge”，是因为在 Jekyll 官网中也曾提到以后将会使用该插件。 
安装步骤非常简单，同样使用命令行安装就OK：

 gem install rouge


Chocolatey的安装
要安装Chocolatey很简单，需要以管理员权限打开命令提示符窗口，然后输入以下命令即可：

 @powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin


Nokogiri软件包安装Permalink

github-pages运行时需要Nokogiri这个软件包，但是要运行在64位Windows系统上还需要执行以下命令：

注意: 在当前版本 pre release 中提供了64位Windows系统支持，但是github-pages中并没有引用这个版本。

choco install libxml2 -Source "https://www.nuget.org/api/v2/"
choco install libxslt -Source "https://www.nuget.org/api/v2/"
choco install libiconv -Source "https://www.nuget.org/api/v2/"

 gem install nokogiri --^
   --with-xml2-include=C:\Chocolatey\lib\libxml2.2.7.8.7\build\native\include^
   --with-xml2-lib=C:\Chocolatey\lib\libxml2.redist.2.7.8.7\build\native\bin\v110\x64\Release\dynamic\cdecl^
   --with-iconv-include=C:\Chocolatey\lib\libiconv.1.14.0.11\build\native\include^
   --with-iconv-lib=C:\Chocolatey\lib\libiconv.redist.1.14.0.11\build\native\bin\v110\x64\Release\dynamic\cdecl^
   --with-xslt-include=C:\Chocolatey\lib\libxslt.1.1.28.0\build\native\include^
   --with-xslt-lib=C:\Chocolatey\lib\libxslt.redist.1.1.28.0\build\native\bin\v110\x64\Release\dynamic


 安装 github-pagesPermalink
 打开命令行界面安装 Bundler: gem install bundler
 打开自己博客的根目录

   jekyll new my-awesome-site   //将my-awesome-site这个博客生成到当前目录下
   cd my-awesome-site   //进入博客根目录
   jekyll serve  //启动服务
