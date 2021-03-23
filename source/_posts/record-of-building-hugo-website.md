---
title: hugo静态网站搭建记录
date: 2019-10-06 19:56:51
categories:
- 博客
tags:
- hugo
- 博客
---

## 搭建介绍

在Windows10下用hugo和GitHub Pages搭建个人静态网站

## 搭建hugo环境

在GitHub上下载hugo的压缩包，在[https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases)下载最新版的Windows-64bit.zip即可。我们假设`$hugo`为hugo所要存放的目录（比如`$hugo`可以为`C:\hugo`），将下载的zip压缩包解压到`$hugo\bin`下，然后把`$hugo\bin`（本文中的例子为`C:\hugo\bin`）添加到Windows环境变量的系统变量Path中。

从[https://git-scm.com/](https://git-scm.com/)上下载git bash，并搭建git环境，使得你本机可以通过git bash连接到你的github库，具体步骤可参考其他git和GitHub教程。

在git bash中输入`hugo help`，若正常显示`hugo`的使用指南即表示hugo环境搭建成功。

## 生成hugo网站

在git bash中，跳转到`$hugo`目录下（以下用`/c/hugo`代替`$hugo`），即使用命令`cd /c/hugo`，然后执行`hugo new site Sites`，即会生成一个名叫`Sites`的文件夹（你也可以将`Sites`替换为其它你想使用的文件夹名），里面包括了你的网站。

由于hugo默认并没有搭载主题，所以需要先使用一款主题才可以成功搭建网站，本文以[Hyde](https://themes.gohugo.io/hyde/)主题为例。在目录`/c/hugo/Sites/`下，使用命令`git clone https://github.com/spf13/hyde.git themes/hyde`，就把Hyde主题下载到了本地。

然后在`/c/hugo/Sites/config.toml`中，修改第一行baseURL为`baseURL = "http://xxx.github.io/"`，并添加一行`theme = "hyde"`。

同时还可以在后面添加网站描述：

```toml
[params]
  description = "Your custom description"
```

更多配置可以参考[Hyde主题的首页](https://themes.gohugo.io/hyde/)。以上，Hyde主题暂时就配置好了。

然后在目录`/c/hugo/Sites/`下使用命令`hugo new about.md`来发布一个页面（发布文章的命令为`hugo new post/about.md`）。然后在about.md的最后添加一行`# about me`（其他markdown语法的内容也可以，这些就是你要写在这个页面的内容）。这样我们就写好了一个页面。

然后可以先生成这个网站预览一下`hugo server --buildDrafts`，其中`--buildDrafts`作用是生成被标记为【草稿】的文件，因为我们的about.md中有`draft: true`，表示这篇文章是一个草稿。生成之后可以在浏览器中输入`localhost:1313`来预览网站，没有问题后，按`ctrl+c`结束预览。

## 推送到GitHub Pages

首先我们需要把我们想要显示的文章和页面（比如上文的about.md）中的`draft: ftrue`改为`draft: false`。

然后在`/c/hugo/Sites/`目录下执行命令`hugo`，此时会生成一个名为public的文件夹，这个文件夹就是我们的网站内容。

在GitHub中新建`xxx.github.io`的仓库（`xxx`为你的GitHub的账户名），然后在Settings中找到Github Pages选项, 将Source改为master branch，最后点击 Save 按钮。这种情况下，该仓库应该为空仓库。

打开`c/hugo/Sites/public`，执行以下命令：

```shell
git init
git remote add origin git@github.com:xxx/xxx.github.io.git
git add -A
git commit -m "Initial"
git push -u origin master
```

以上，网站就生成了，过几分钟后访问xxx.github.io，如果访问成功，说明搭建成功。

之后的日常发布为：

```shell
git add -A
git commit -m "update"
git push -u origin master
```

## 番外

笔者实践时，因为之前已经搭建过一次静态网站了，该仓库中已经有了内容，所以需要先将其删掉。经实践后，命令如下：

```shell
cd /c/hugo/Sites
git clone git@github.com:xxx/xxx.github.io.git
cd xxx.github.io
git rm -rf *
git commit -m "empty"
git push origin master
cd ..
rm -rf xxx.github.io
```

通过以上命令，先将仓库的文件都删掉。然后执行以下命令：

```shell
cd /c/hugo/Sites
hugo
cd pubilc
git init
git remote add origin git@github.com:xxx/xxx.github.io.git
git pull origin master
git add -A
git commit -m "Initial"
git push -u origin master
```

才能完成网站的搭建。