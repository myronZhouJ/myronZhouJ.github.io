---
layout: post
title:  "升级到OS X EI Capitan 后通过gem安装的所有工具不能使用了 -command not found 错误的问题"
date:   2015-12-03 15:42:44
categories: jekyll update
---
mac系统升级到EI Capitan 后，所有通过gem安装的工具均不能再使用，出现command not found的错误。

试图重装gem但出现权限错误，加sudo后，错误依旧，网上看了下，是因为Apple的System Integrity Protection (SIP) 特性。

以前的ruby在/usr/bin/目录下，现在把他从新安装到/usr/local/bin目录中:

$ sudo chown -R $(whoami):admin /usr/local

$ brew update

$ brew install ruby

$ gem install jekyll

详细的可以看文章：http://www.hacksparrow.com/os-x-el-capitan-screwed-up-ruby-gems-and-how-to-fix-it.html
