---
layout: post
title:  "利用github做服务器搭建个人博客"
date:   2015-08-06 15:42:44
categories: jekyll update
---
搭建Blog的过程和遇到的问题（假定你会git，环境mac10.10）

开始搞定jekyll，不管是什么，按照网上的教程先干了再说

1.检查是否已经安装ruby，否则安装ruby

ruby -v(查看ruby版本)

2.安装jekyll

sudo gem update —system（更新gem，查了下gem好像是软件包管理的）

sudo gem install jekyll 

3.安装模板

sudo gem install rdiscount

4.初始化jekyll环境

在你希望的任何地方创建一个你可以随便命名的文件夹用来存放将来写的东西（我随便创建了一个叫Blogs的文件夹），然后cd进入这个文件夹后执行命令：

jekyll new blog

执行后你就看到，在Blogs里面有一个名为blog的文件夹，里面就可以看到一个jekyll的结构，具体的后面说吧。

5.测试jekyll

先开启jekyll服务命令：jekyll serve(注意：执行这个命令一定要在确保当前是在blog目录中，否则会出错)

如果没问题，那么你在浏览器输入http://localhost:4000，将看到welcome页面。


下面执行github相关的，请确定你已经安装了git，否则安装，具体的这里不说吧

注册账号，然后创建一个名为“yourAccountName.github.io”的仓库,这我就不详细说了，大家应该会，注意替换yourAccountName为你在github上的名字

1.在本地创建一个仓库文件夹，我在Blogs中创建了一个GitBlogsRepo的文件夹

2.确保你现在处在GitBlogsRepo文件夹后，克隆你在github上创建的那个仓库到本地

git clone https://github.com/myronZhouJ/myronZhouJ.github.io.git（注意替换成自己的AccountName）

执行上面后，我的GitBlogsRepo中看到myronZhouJ.github.io的文件夹

3.HelloWorld

cd到myronZhouJ.github.io后,创建一个index.html的文件,在里面写入HelloWord

cd到myronZhouJ.github.io,一步步执行下面命令：

git add . (注意点“.”)

git commit -m “Test Hello World”

git push

上面完成后，在浏览器输入：http://myronzhouj.github.io，将看到Hello word.

整合jekyll和github，很简单

将上面的blog文件夹中的所有东西，复杂到myronZhouJ.github.io文件夹,然后执行上面类似的add，commit，push。

最后你在浏览器输入myronZhouJ.github.io，你会看到那个welcome页面.

估计以后每次在myronZhouJ.github.io中修改jekyll中的东西或者文章后都要：add,commit,push

jekyll知识，然后开始写文章了

修改myronZhouJ.github.io文件夹中的_config.yml.具体的要看看文档，就是一些网站基本信息title，description和配置等等,不在这里说了，我先不对这个文件做修改，下面写个文章，至于文章内容就是你目前正在看的这个了.哈哈

以后博文就在myronZhouJ.github.io文件夹中的_posts文件夹,

我先直接修改welcome的例子，就可以了.然后add，commit，push。

详细jekyll的知识，我还需要慢慢学习。

如果你想用自己的域名而非userName.github.io，只需要做个设置，相当方便，方法这里就不说，因为我没得域名的，不会。
