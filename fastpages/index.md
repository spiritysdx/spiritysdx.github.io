# fastpages博客搭建指南


## 前言

fastpages博客只适用于GitHub托管。

fastpages博客没有主题选择，想要对页面进行修改，需要有前端html，css，JavaScript语言基础，直接对模板进行修改，各个页面模板在layout文件夹下，主页模板是根目录的index.html文件，需要修改主题可以自行修改。

fastpages缺点就是上述部分，优点如下：

1.本地不需要搭建站点，只需要按照格式命名文件，并在文件内容头部加上头部信息。

2.可以自定义添加评论区，方法简单。

3.支持jupyter notebook文件，markdown文件，word文件。

4.云端后端服务自动将上传的文件转化为html文件展示到博客上。

5.内置搜索。

6.可以嵌入Twitter卡和YouTube视频。

## 简单创建步骤

声明：源步骤链接--->>>[原说明](https://github.com/fastai/fastpages/blob/master/README.md)

本人将说明简化如下，如有遗漏，请阅读原说明。

[视频教程点击这里跳转](https://www.bilibili.com/video/bv1rp4y1W741)

该教程请配合视频-->>>   使用，如有错漏或疑问，评论区留言我会解答。

1.登陆GitHub账户，在保持登陆的情况下复制下面链接到浏览器打开创建分支---->>>`https://github.com/fastai/fastpages/generate`

2.命名仓库名时不能使用格式为`你的用户名.github.io`

3.创建完成后等待actions选项中的进程都变为绿色后再进行下一步操作

4.完成上面选项后复制下面网址按照视频提示加载SSH keys---->>>链接：`https://8gwifi.org/sshfunctions.jsp`

5.按照视频教程在`Settings`中的'Secrets'中添加私钥，在'Deploy keys'中添加公钥

6.等待actions选项中的进程都变为绿色后再进行下一步操作

7.上述步骤完成后在```Pull requests```中合并分支，点击下面绿色的`merge pull request`合并

8.首页README.md中显示你的博客地址，可以点击查看

9.按照视频教程修改`_config.yml`文件

10.按照视频教程修改`index.html`文件

11.按照视频教程修改`pages`文件夹中的`about.md`文件

12.写博客只需要在本地写好博客，md文件命名格式为`2020-01-28-文章标题写这里.md`，md文件内容头部信息格式如下：

```
---
toc: true
layout: post
description: 文章概述
categories: [markdown]
title: 标题
---
```

13.jupyter notebook文件命名格式为`2020-01-28-文章标题写这里.ipynb`，word文件命名格式为`2020-01-28-文章标题写这里.docx`

14.只有md文件需要头部信息，其他文件不需要

15.上传不同格式的文档注意上传的文件夹是不同的，word文档上传到`_word`文件夹里，md文档上传到`_posts`文件夹里，jupyter notebook文档上传到`_notebooks`里

15.等待actions选项中的进程都变为绿色后则部署完成，刷新博客页面即可展示修改结果

## 示例博客

[fastpages](https://spiritlhl.github.io/jupyter_notebook_blogs/)

