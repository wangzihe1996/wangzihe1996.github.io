---
title: 校园网ipv6免流方法
date: 2019-10-06 20:28:35
categories:
- ["校园网"]
tags:
- ipv6
- 校园网
---

前段时间路由器的ipv6有问题，修了几个晚上都没有修好。现在发现它自己又好了，就记录一些自己折腾的一些心得吧。

## 适用范围

校园网收费，需认证登录。但是ipv6可以免费使用。

<!-- more -->

## 检查ipv6是否可用

1. 在网络和共享中心中可以查看自己的状态，查看ipv4连接和ipv6连接是否可用，以及ip地址，在Internet协议版本6中也可以修改dns地址。关于ipv6的dns服务器可以去网上找。
2. 在cmd中用ipconfig和ipconfig/all也可以查看ipv4和ipv6地址。可以通过ping一些ipv6的网址来检查本机ipv6是否可用。只有ipv6解析的网址：[ipv6.google.com](ipv6.google.com)、[ipv6.l.google.com](ipv6.l.google.com)、[ipv6.test-ipv6.com](ipv6.test-ipv6.com)，以及ipv6解析和ipv4解析都有的网址：[ipv6.tsinghua.edu.cn](ipv6.tsinghua.edu.cn)、[ipv6.ustc.edu.cn](ipv6.ustc.edu.cn)、[ipv6.sjtu.edu.cn](ipv6.sjtu.edu.cn)。注意有些学校虽然有ipv6但是国外的网址（如Google、YouTube等）会屏蔽掉，换句话说也就是，ipv6也已经有墙了。虽然Google等用ipv6可以直连但是你发现自己无法连接，也ping不通。ipv6.test-ipv6.com这个网址目前没有被屏蔽，只有ipv6解析，是一个比较好的检测方式。
3. 以上说的网址，除了可以检查是否能ping通外，也可以在浏览器中直接打开，看看能不能打开。[test-ipv6.com](test-ipv6.com)是一个检查ipv6可用性的网站，但是它的ip地址是ipv4的，所以可以用它的ipv6版本：[ipv6.test-ipv6.com](ipv6.test-ipv6.com)。以及北邮人TV：[tv.byr.cn](tv.byr.cn)。

## ipv6免流方法

在确认本机ipv6可用后，就可以用它来免流了。首先要先拥有一个有ipv6地址的服务器（如vultr、aws和搬瓦工等等）。然后在上面搭建ss/ssr服务器。ssr是经实践可用的，至于ss是否可用可以自行测试。
搭建好ssr服务器后，就可以走代理来免流了。几个平台的免流方法如下：

1. Windows平台：下载ssr客户端，然后添加ssr服务器，ip地址写ipv6地址即可。开全局代理后，浏览器可以正常上网，但是其他软件不一定能使用。有些软件可以走代理，有些不能。所以需要把ss作为电脑的全局代理。一种方法是用Proxifier，但是它并不完美，有些软件不一定能使用，而且貌似也不能转发udp。因此可以使用ss-tap这个软件，它本来是为游戏做加速的，但是我们也可以使用，由于官方网站上的https://www.sockscap64.com/zh-hans/sstap/ 是最新版本1.1.0.1，不支持全局代理，因此需要下载最后一个支持全局代理的版本1.0.9.7，这里提供一个下载地址https://github.com/mayunbaba2/SSTap-beta-setup/blob/master/SSTap-beta-setup-1.0.9.7.exe.7z 。安装ss-tap后，由于其本身并不能添加ipv6地址的代理，因此需要本地打开ssr，然后ss-tap中添加本地端口，即类型：socks5，地址：127.0.0.1，端口：1080。如此之后，Windows电脑即可实现全局免流上网（Windows应用商店可能无法打开，原因未知）。
2. 安卓平台：手机端较为简单，下载ssr客户端后，添加代理服务器，ip地址填ipv6地址即可。连接校园网WiFi后，打开ssr，连接全局代理即可（有时可能需要连接WiFi一段时间后才能正常使用）。
3. 路由器平台：个人比较喜欢的方式当然是搭一个可以免流的路由器，然后手机电脑平板等设备都可以连到路由器来上网，这种方式也叫透明代理。因此需要有一个支持ipv6和ssr的路由器，然后ssr添加ipv6的代理服务器并选择全局模式即可。使用路由器后，电脑和手机均无需配置即可免流上网，而且一些小问题在这种模式下也不会出现，体验较为良好，当然有兴趣的也可以自己用树莓派搭建。

## 更新

前阵子迎来了一个小长假，同时由于该时期的特殊性，大量的ssr服务器被封掉，笔者亲测一个新部署的ssr服务器使用不到一天就会被封，因此我怀疑ssr的特征识别率已经很高了，所以我觉得ssr不应该被使用了，可以考虑用v2ray。

不过比较幸运的一点是，通常只会封ipv4地址，而不会封ipv6地址，即使你的服务器的ipv4地址被封了，ipv6地址大概率还是可用的，因此用ipv6来免流或者科学上网还是有可行性的，当然前提是你本地要有ipv6网络。

以上就是我折腾校园网免流的一些经验，欢迎交流指正。