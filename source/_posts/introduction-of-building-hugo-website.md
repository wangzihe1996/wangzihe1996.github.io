---
title: hugo静态网站部署到GitHub Pages并实现多平台更新
date: 2019-10-06 20:06:03
categories:
- 博客
tags:
- hugo
- 博客
---

本文作为[hugo静态网站搭建记录]()的进阶版，主要是考虑到了多平台的问题。由于我们没有自己的服务器，网站托管在Github上，如何在其他平台上更新自己的网站就成为了一个问题。因此按照[官网的指导](https://gohugo.io/hosting-and-deployment/hosting-on-github/)，写了本篇文章，把网站的源代码文件和可执行文件都存储到GitHub的仓库中。当我们更换了一个平台时，只需把源代码文件从GitHub上pull下来（二进制文件作为该仓库的子模块），即可写文章并推送到博客上。

<!-- more -->

如果你还没有了解过hugo，建议先看我之前的文章。

首先，在github中建立两个名为`blog`和`xxx.github.io`的仓库，前者用来存储源代码文件，后者用来存储可执行文件。

网站的生成命令如下：

```shell
hugo new site blog
cd blog
git init
git remote add origin git@github.com:wangzihe1996/blog.git
git pull origin master
git clone https://github.com/spf13/hyde.git themes/hyde
hugo new about.md
hugo new post/test.md
```

在`about.md`和`test.md`中添加一些内容用来测试，同时设置draft为false。

修改`config.toml`配置文件，修改第一行baseURL为`baseURL = "http://xxx.github.io/"`，并添加一行`theme = "hyde"`，更多配置可以参考[Hyde主题的首页](https://themes.gohugo.io/hyde/)

然后使用`hugo server`命令，来生成网站，打开`localhost:1313`来查看有没有问题，测试没有问题后，用`ctrl+c`来终止测试。

然后把网站的源代码文件和可执行文件推送到GitHub的仓库中。

```shell
cd /c/hugo/blog
git submodule add -b master git@github.com:wangzihe1996/wangzihe1996.github.io.git public
hugo
cd public
git add -A
git commit -m "init&example"
git push -u origin master
cd ..
git add -A
git commit -m "init&example"
git push --recurse-submodules=check -u origin master
```

以后的日常推送为：

```shell
cd /c/hugo/blog
hugo
cd public
git add -A
git commit -m "update"
git push -u origin master
cd ..
git add -A
git commit -m "update"
git push --recurse-submodules=check -u origin master
```

