---
title: 打开新世界 - 利用你的 VPS  实现 IPv4 访问 IPv6 网站
date: 2017-08-20 20:52:00
tags: [Linode, VPS, IPv4, IPv6]
categories: [教程]
banner: https://dfjvma.dm2301.livefilestore.com/y4mCBemoAc6MPUlLwKndkAgi5IABFHlJiejxJoCH7_cz444J2XEi0Z1ctaND6yZMMnF-8WLLgXA_c0osc2t6uBJQTo2fO5lx_N8ih5aHoDZCYitlnLT-c1G4G4_84isGrjgvWa58aibk3k0RBT66vh3of05qJlUXA5iUGBLBQUeOlEoob4pvsqr5c1Z5mHBNms4OXU58eERajHsmewYqu0McA?width=1280&height=582&cropmode=none
thumbnail:
---
IPv6 的概念已经出来很久，但随着各种因素，比如 NAT 的广泛使用等，IPv6 的发展似乎停滞不前，平时也几乎感觉不到它的存在。虽然一直都有电信 IPv6 试商用的消息，可目前对它支持最好的还只是教育网。那我们这些早已毕业离校的学渣就从此告别 IPv6 了吗？作为一位新技术爱好者，当然要好好研究一番。<!--more-->

首先引起我注意的，自然是 VPS 的信息里赫然列着的 IPv6 几个字。顺藤摸瓜，果然找到了很多这方面的教程与文章。这里的内容也是我对这些前辈给出方法的实践总结。

#### 获得你的 IPv6 地址

这一步不是必须，因为如果只是实现最普通的 IPv4 访问 IPv6，而你已经安装好 SS 的话，只要修改服务器上一个参数即可，本地可以不做任何修改。若是你本地也支持 IPv6 的话，则可以使用这个地址实现 IPv6 访问“那些不存在的网站”。

主流 VPS 服务商都已经支持 IPv6。Linode 的地址在 `remote access` 标签中，Vultr 则需要在部署服务器时的 `Additional Features` 中勾选 `Enable IPv6` 。而搬瓦工（Bandwagon）比较特殊，需要在服务器的 KiwiVM 控制面板中的 `IPv6 Addresses` 中开启，而且可以分配的 IPv6 数量很少，不过这对一般使用没有任何影响。

#### 让你的 SS 支持 IPv6

首先你必须安装了 SS 服务，只有这样，VPS 才能成为跳板，这和我们访问“那些不存在的网站”是一个道理。

修改 SS 的配置文件，将服务器地址改为 `::`，示例

```php
  {
      "server": "::",
      "server_port": 1234,
      "password": "12345678",
      "method": "cccccc",
      "remarks": "",
      "auth": true,
      "timeout": 5
    }
```

这样，你的 SS 就可以访问 IPv6 的地址了。

如果您本地支持 IPv6 的话，还可以在 SS 客户端新建一个地址为服务器 IPv6 地址的服务（有点多此一举，因为你本来就可以直接用 IPv6，除非访问”那些懒得打字了“），如果不支持，就用原来的 IPv4 地址，无需任何改动。

试试访问 `ipv6.google.com` 或者 `bt.byr.cn`，这时你可能需要在 SS 客户端开启全局访问，或者把需要的地址添加到规则里。用 `ipv6-test.com` 测试得到的将是你 VPS 的 IP 和地址。

#### 另一个方法

如果你有公网 IP，路由器又支持 IPv6 的话，还可以用 [Hurricane Electric](http://he.net/ "Hurricane Electric") 的服务，这样就不需要 VPS 了，具体方法请参考 [TARESKY](https://taresky.com/post/ipv6 "TARESKY") 的分享。

其实这个方法才是比较有意义的，但 NAT 的存在使得拥有公网 IP 的人越来越少， HE 的服务也不是很稳定，关于这方面的内容后面有机会再分享。

**Tip：怎么确认自己有没有公网 IP**

万网有一个[测试本地 IP](http://www.net.cn/static/customercare/yourip.asp "测试本地 IP") 的页面，如果这里输出的 IP 是两个，一般后面一个还是 `10` 之类的开头，那么恭喜你，和我一样没有公网 IP。

#### 最后

对于我个人来说，总是希望看到各种强大，让人生活更便捷的新技术快速应用和普及，但因为各种各样的原因，现实总是常常相反，很多实用的技术最终沦为小众甚至消亡，令人唏嘘。希望 IPv6 早日普及。

以上，是关于如何实现 IPv4 访问 IPv6 的简单分享。

#### 邀请链接
算是小广告，Linode 的使用体验很好，目前最便宜的服务器 5 美元/月，能满足普通人大部分折腾需求，如果你感兴趣，可以通过[这个邀请链接](https://www.linode.com/?r=94b59a4c8e42daff9649bde1f60abee7f435a108 "这个邀请链接")注册，或填写这个邀请码（referral code）：
`94b59a4c8e42daff9649bde1f60abee7f435a108`
~~你啥也得不到，但我可以从中受益~~

**Enjoy!**
