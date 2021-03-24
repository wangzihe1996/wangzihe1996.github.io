---
title: 用Hexo和NexT来搭建Github Pages静态博客
date: 2019-10-12 21:11:24
categories:
- 博客
tags:
- hexo
- 博客
---

## 为什么选择Hexo和NexT

前阵子花了不少时间解决了hugo下公式显示的问题，终于把一个博客搭了起来。然后我发现，**不如Hexo+NexT的博客好看**，这是我再一次更换博客框架的原因之一，虽然很多人的博客主题都是NexT导致我已经有些审美疲劳了，但是它确实是一个很简洁优雅的主题。其次，Hexo是目前最流行的静态博客框架，NexT是Hexo中使用人数和star数最多的主题，这两者的功能都异常强大，足以满足我的需要，并且文档以及民间资料都很丰富，绝大部分功能都只需要更改配置文件即可，一小部分需求通过在网上查找别人的方法也能解决。

hugo作为一个新生的博客框架，虽然它的优点显而易见，但是功能还不够丰富，一个功能比较健全的主题[Academic](https://themes.gohugo.io/academic/)不符合我的审美，而且似乎想要配置成功也不太容易。我认为我搭博客的目的是能够方便地总结、记录和分享自己所学所做，而不是每天来手动实现博客的各种功能。因此，我的博客重新使用了Hexo+NexT来搭建。

<!-- more -->

## 我安装的npm包

1. MathJax，[使用方法](https://theme-next.org/docs/third-party-services/math-equations)

   ```shell
   npm un hexo-renderer-marked --save
   npm i hexo-renderer-pandoc --save
   ```

2. 站内搜索(Local Search)，[使用方法](https://theme-next.org/docs/third-party-services/search-services#Local-Search)

   ```shell
   npm install hexo-generator-search --save
   ```

3. GitHub部署插件，[使用方法](https://hexo.io/zh-cn/docs/one-command-deployment#Git)

   ```shell
   npm install hexo-deployer-git --save
   ```

4. 图片插件，[使用方法](https://github.com/xcodebuild/hexo-asset-image/)

   ```shell
   npm install https://github.com/xcodebuild/hexo-asset-image.git --save
   ```

## Hexo无痛使用本地图片

Hexo的[官方文档](https://hexo.io/zh-cn/docs/asset-folders)中提供了几种上传本地图片的方法，但是这种标签插件的方式并不直观，因为它不是Markdown语法，所以图片不能在Typora上显示，而使用`source/image`文件夹的方法也同样由于路径不同的问题而使得图片不能在Typora上显示。使用`post_asset_folder`属性和`hexo-asset-image`插件可以解决这个问题。

解决方案：首先在Hexo的`_config.yml`文件中设置`post_asset_folder: true`，然后安装插件`npm install https://github.com/xcodebuild/hexo-asset-image.git --save`。由于`hexo-asset-image`插件有bug，因此需要尝试不同版本来看看哪个版本可以解决问题。

我的hexo的版本为3.9.0；hexo-cli的版本为3.0.0；操作系统为Windows 10(64bit)；NexT主题的版本为[7.4.1](https://github.com/theme-next/hexo-theme-next/releases/tag/v7.4.1)。Hexo的`_config.yml`的配置中`url`为https://username.github.io；`root`)为`/`；`permalink`为`:year/:month/:day/:title/`；`permalink_default`为空，`post_asset_folder`为`true`，其它相关设置为Hexo的默认值。我使用成功的`hexo-asset-image`版本为[https://github.com/xcodebuild/hexo-asset-image/tree/3c114cf0c0343ab28469635085b225fcae7fb9d3](https://github.com/xcodebuild/hexo-asset-image/tree/3c114cf0c0343ab28469635085b225fcae7fb9d3)。

采用以上方法后，每使用`hexo new title`时，在生成`title.md`时也会生成一个名为`title`的文件夹。你可以把你在`title.md`中要使用的图片（例如`test.jpg`）放到title文件夹中。然后在使用相对路径的方法插入该图片`![](title/test.jpg)`。如此，在生成的静态博客中，该图片会以绝对路径的形式出现，因此不论是在首页还是在文章页面都能正常显示。

另外，Markdown中插入图片的语法为`![Alt text](图片链接 "optional title")`，但是在Hexo+NexT下，text似乎也会被显示出来，即使图片显示成功，不太美观，因此`Alt text`建议不要填写，至于`"optional title"`，没有测试，并不清楚。

> Alt text：图片的Alt标签，用来描述图片的关键词，可以不写。最初的本意是当图片因为某种原因不能被显示时而出现的替代文字，后来又被用于SEO，可以方便搜索引擎根据Alt text里面的关键词搜索到图片。 图片链接：可以是图片的本地地址或者是网址。"optional title"：鼠标悬置于图片上会出现的标题文字，可以不写。 

## tags、archives、categories和about

对于使用`hexo new page test`建立的页面，会有date和title两者属性，但是title似乎是不需要的，至少[7.4.1](https://github.com/theme-next/hexo-theme-next/releases/tag/v7.4.1)版本的NexT主题是不需要的，而且有的话在页面还会额外多一个标题，很不美观。因此可以删掉title属性，同时添加type属性(并不确定type属性是否有效，但是添加后不会产生错误)。

例如`source/tags/index.md`可以写成如下内容：

```yaml
---
date: 2019-10-09 21:19:36
type: tags
---
```

## 分类问题

Hexo中默认一篇文章只有一个分类属性，不过可以使用父子类别。具体使用方法见Hexo的[中文文档](https://hexo.io/zh-cn/docs/front-matter#分类和标签)和[英文文档](https://hexo.io/docs/front-matter.html#Categories-amp-Tags)。

```yaml
categories:
- Diary
tags:
- PS3
- Games
```

根据文档中可以知道，单分类的情况如上，`categories`下只指定一个分类。

```yaml
categories:
- Sports
- Baseball
tags:
- Injury
- Fight
- Shocking
```

父子分类的方法如上，Sports分类下还有一个名为Baseball的子分类，这篇文章属于Baseball这个二级分类和Sports这个一级分类。

```yaml
categories:
- [Sports]
- [Daily]
tags:
- Injury
- Fight
- Shocking
```

以上形式可以使用多分类，这篇文章既属于Sports分类又属于Daily分类，而且这两个分类是平行关系。

```yaml
categories:
- [Sports, Baseball]
- [MLB, American League, Boston Red Sox]
- [MLB, American League, New York Yankees]
- Rivalries
```

这种形式可以既使用多分类又使用平行分类，这篇文章属于Sports、MLB和Rivalries这三个一级分类，同时属于Sports下的Baseball和MLB下的American League这两个二级分类，同时还属于MLB下的American League下的Boston Red Sox和New York Yankees这两个三级分类。

如果你遇到空格的显示问题，我觉得把内容用双引号引起来应该会解决问题，例如这样`"American League"`。

标签方面，Hexo是支持多标签的，同时标签之间是平行关系，也没有父子关系，所以`tags`属性正常写就可以啦。

## 常用命令

1. 本地查看

   ```shell
   hexo clean && hexo g && hexo s
   ```

2. 部署到GitHub Pages

   ```shell
   hexo clean && hexo g && hexo d
   ```

## 必须联网？

在前两天的一次使用中无意中发现，似乎是由于某个npm包的原因，即使只使用`hexo clean && hexo g && hexo s`命令在本地查看，也需要联网才行，否则似乎会在`hexo g`的时候报错。

## 多电脑同步：

思路很简单，就是把你的博客源代码文件也上传到GitHub上，可以上传到你的`username.github.io`的非master分支上，也可以新建一个仓库。考虑到Google Analytics的id等私人信息也会被上传上去，因此可以建立一个private仓库来存源代码。

其实是Google时找到了[一篇博客](https://segmentfault.com/a/1190000017784822)，参考了其中的思路。

主要有两个问题，一个是你的源代码里的主题和其他插件里都有`.git`文件和`.gitignore`文件，`.git`文件会使得该文件夹的内容不会被同步，应该是GitHub认为这是一份别人的仓库文件吧（事实上也确实是从别人的仓库里clone来的）。`.gitignore`文件会使得某些文件在同步时被忽略，一些字体文件之类的会在里面，然后你在另一条电脑从GitHub上pull下来后运行时就会发现有问题，缺少文件。

后来我发现，我的整个博客文件夹才50MB，在9102年，这个体积很小的好嘛，完全上传到GitHub上都没有问题。所以第一个问题的解决方法很简单，在你的源代码文件夹中搜索`git`，把除了`.deploy_git`文件夹之外的其它`.git`文件和`.gitignore`都删掉。

第二个问题是node_modules问题，因为下载了一些npm包在博客文件夹中，但是因为文件夹整体大小也很小，所以我觉得，就按上述办法，把它们全部上传也没什么不妥。

还有就是`.deploy_git`文件夹，这个是Hexo用来上传博客程序的，你可以把它加到`.gitignore`里，也可以不管它，反正它的体积也很小。

```shell
git init
git remote add origin git@github.com:username/blog.git
git add -A
git commit -m "init"
git push -u origin master
```

所以就是新建一个private仓库，把博客的源代码上传上去。