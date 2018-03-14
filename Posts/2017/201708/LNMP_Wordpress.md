---
title: 使用 LNMP 快速部署 Wordpress 网站的经验分享
date: 2017-08-18 23:43:00
tags: [Linode, LNMP, Wordpress]
categories: [教程]
banner: https://ur.img.com
thumbnail: https://ur.img.com
---
**[LNMP](https://lnmp.org/ "LNMP")　及代表 Linux/Nginx/MySQL/PHP，与之对应的还有 [LAMP](https://lamp.sh/ "LAMP")（A 代表 Apache）。两者都能实现一键快速部署。**

#### 为什么采用 LNMP 这种架构?
采用 Linux、PHP、MySQL 的优点我们不必多说。

Nginx 是一个小巧而高效的 Linux 下的 Web 服务器软件，是由 Igor Sysoev 为俄罗斯访问量第二的 Rambler.ru 站点开发的，已经在一些俄罗斯的大型网站上运行多年，目前很多国内外的门户网站、行业网站也都在是使用 Nginx，相当的稳定。<!--more-->

Nginx 相当的稳定、功能丰富、安装配置简单、低系统资源 ...

#### 安装非常简单

因为 LNMP 的安装是傻瓜式的，这里直接给出官网的教程，不再多此一举抄一遍。主要说说自己安装过程中的一些经验，这里假设你已经知道了如何使用 SSH 等基础知识，如果这些都不懂 ... 当我没说。

1. [安装 LNMP](https://lnmp.org/install.html "安装 LNMP")
2. [添加虚拟主机](https://lnmp.org/faq/lnmp-vhost-add-howto.html "添加虚拟主机")

#### 我的经验
##### 上传网站
首先准备好需要上传的网站内容和上传工具。如果是全新网站，可以直接去 [Wordpress 官网](https://cn.wordpress.org/ "Wordpress 官网")下载最新的安装包。上传有 FTP 和 SFTP 两种方式，推荐使用 SFTP 方式，直接下一个免费的 SFTP 工具（比如 [FileZilla](https://filezilla-project.org/ "FileZilla")），用服务器的 SSH 账户登录即可。若用 FTP，则需先在服务器[安装 ftp 服务](https://lnmp.org/faq/ftpserver.html "安装 FTP 服务")，安装完成再用 FTP 客户端登陆。

我自己用的客户端是 [Total Commander](http://www.ghisler.com/ "Total Commander")，这是一款付费软件，但功能真是强大，FTP 还是 SFTP 都能实现。

推荐先将网站文件打包再上传，速度会快很多，一般默认路径是 /home/wwwroot，上传结束后进入目录解压。

```php
cd /home/wwwroot *改成你自己的路径即可
unzip wordpress.zip *改成你自己的压缩包
```

解压结束后别忘了把文件名改成之前设置的网站域名。（目录下应该已经有这个名称的文件夹，可以先把这个文件夹改成其它名字）

##### 修改文件夹权限
网站文件传好后记得改一下文件夹权限，否则后面在后台进行安装插件等操作时非要你登陆 FTP，修改权限也巨简单

```php
sudo chown www:www -R /home/wwwroot/www.isky.io
```
路径和网站改成你自己的即可。

##### 修改数据库文件夹名称
在 wwwroot 下还有个 default 文件夹，里面存放的是欢迎页面和数据库相关的页面文件，留下 `phpmyadmin` 和 `.user.ini.` 其它全部删掉。再将 `phpmyadmin` 改成你自己知道的名字，这样，访问数据库就可以通过 `你的域名/你自己知道的名字` 进行。

##### 看不到主题怎么办？
通过 LNMP 安装的 Wordpress 经常遇到只能看到默认主题的情况，反正我是每次都遇到 ...

只要删除 `/usr/local/php/etc/php.ini` 中的 `scandir` 这个单词即可解决。

##### HTTPS 支持
你可以自己到阿里云之类的地方给自己的域名申请免费证书，把下载的证书上传到服务器。在下文中提到的配置文件中填写证书路径即可，格式类似 `/home/wwwroot/你的证书文件名+后缀`。

###### 示例
```php
ssl_certificate /home/wwwroot/12345678.pem;
ssl_certificate_key /home/wwwroot/12345678.key;
```

当然前提是你安装虚拟主机时选择了安装 SSL（LNMP 1.4 版本新增功能，非常实用，安装时可以先随便写一个路径，后面再改）。

##### HTTP 重定向到 HTTPS
主机配置文件路径是 `/usr/local/nginx/conf/vhost`，你可以在此找到对应域名的配置文件，重定向也在此修改。找到如下内容，添加一行代码

```php
server
    {
        listen 80;
        #listen [::]:80;
        server_name isky.io;
        return 301 https://isky.io$request_uri; *添加此行代码，域名自行更换
        index index.html index.htm index.php default.html default.htm default.php;
        root  /home/wwwroot/www.isky.io;
```

暂时想到这些，又需要再更新，欢迎指正。

#### 邀请链接
我的网站目前托管在 Linode 上，使用体验很好，如果你感兴趣，可以通过[这个邀请链接](https://www.linode.com/?r=94b59a4c8e42daff9649bde1f60abee7f435a108 "这个邀请链接")注册，或填写这个邀请码（referral code）：
`94b59a4c8e42daff9649bde1f60abee7f435a108`
~~你啥也得不到，但我可以从中受益~~

**Enjoy!**
