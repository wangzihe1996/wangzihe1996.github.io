---
title: Typecho博客搭建记录
date: 2019-10-06 19:10:17
categories:
- 博客
tags:
- typecho
- 博客
---

博客最终还是从云服务器迁移到了GitHub Pages，框架也从Typecho变成了hugo，这里记录一下Typecho当时的搭建方法吧。

服务器为ubuntu18，部署环境为lamp(linux+apache+mysql+php)。

部署步骤：

<!-- more -->

1. 安装ubuntu18系统，在vps提供的系统镜像中选择ubuntu18，然后点击安装即可。

2. 搭建lamp环境：

   ```shell
   sudo apt update
   sudo apt upgrade
   sudo apt install tasksel
   sudo tasksel install lamp-server
   sudo bash -c "echo -e '<?php\nphpinfo();\n?>' > /var/www/html/phpinfo.php"
   ```

   然后在浏览器中打开你的域名或者ip地址，查看是否有apache的页面，然后再打开你的域名或者ip地址/phpinfo.php，检查其页面是否正常显示。

   以上完成，说明你的lamp搭建好了。

3. 设置mysql：

   ```shell
   sudo mysql_secure_installation
   mysql -u root -p
   ```

   ```sql
   CREATE DATABASE typecho;
   CREATE USER `username`@`localhost` IDENTIFIED BY 'password';
   GRANT ALL ON typecho.* TO `username`@`localhost`;
   FLUSH PRIVILEGES;
   ```

4. 设置apache：

   保持默认应该就可以，可根据需要设置`/etc/apache2/apache2.conf`和`/etc/apache2/sites-available/`中的内容。
   删除原有首页`rm /var/www/html/index.html`，或把`/etc/apache2/mods-enabled/dir.conf`中的`index.php`放在`index.html`之前。

5. 安装php相关：

   这一步我也不确定是否为必需操作。

   ```shell
   sudo apt update
   sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip
   ```

6. 部署Typecho框架：

   ```shell
   wget http://typecho.org/downloads/1.1-17.10.30-release.tar.gz
   tar -zxvf 1.1-17.10.30-release.tar.gz
   mv build/* /var/www/html
   rmdir build
   ```

7. 网站程序安装：

   在浏览器中打开你的域名或者ip地址，

   ![typecho-install](http://docs.typecho.org/_media/wiki/install.png?cache=)

   然后在其中设置你的各项配置，其中数据库地址为localhost，用户名和密码为你创建的用户名和密码（授予了权限的那个），然后就安装完成了。

8. 插件安装：

   官方插件：[https://github.com/typecho/plugins](https://github.com/typecho/plugins)
   民间插件：[https://github.com/typecho-fans/plugins](https://github.com/typecho-fans/plugins)
   官方文档中所列的插件：[http://docs.typecho.org/plugins/download](http://docs.typecho.org/plugins/download)
   **注意**：需将插件文件夹（或文件）放在网站目录下的usr/plugins/下才行，文件夹名称要和插件名称相同

以上，即可完成typecho的安装。