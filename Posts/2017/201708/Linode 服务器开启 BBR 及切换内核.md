---
title: Linode 服务器开启 BBR 及切换内核
date: 2017-08-16 20:10:00
tags: [BBR, VPS, Linode]
categories: [教程]
banner: https://dfjvma.dm2301.livefilestore.com/y4mCBemoAc6MPUlLwKndkAgi5IABFHlJiejxJoCH7_cz444J2XEi0Z1ctaND6yZMMnF-8WLLgXA_c0osc2t6uBJQTo2fO5lx_N8ih5aHoDZCYitlnLT-c1G4G4_84isGrjgvWa58aibk3k0RBT66vh3of05qJlUXA5iUGBLBQUeOlEoob4pvsqr5c1Z5mHBNms4OXU58eERajHsmewYqu0McA?width=1280&height=582&cropmode=none
thumbnail:
---
换到 Linode 的服务器以后，第一件事就是安装 BBR，可是按照之前的方法安装总是失败。后来才发现其实不用那么麻烦，Linode 4.90 版本以上的内核都已经内置了 BBR，但默认是关闭的。

只需要几行简单的命令即可。<!--more-->

#### 切换内核
首先，您可以在自己 VPS 的 Dashboard 中查看内核版本：

![](https://isky.io/wp-content/uploads/2017/08/linode-dashboard.jpg)

如果自己的内核已经高于 4.90 则可以直接进行后面的步骤启用 BBR。否则要切换内核，只要点击右侧的 Edit，重新选择 Kernel 即可，不过记得还要点击页面底部的`确认修改（Save Changes）`按钮，不是直接选了内核就生效的，别问我怎么知道 ...

![](https://isky.io/wp-content/uploads/2017/08/linode-kernel.jpg)

#### 启用 BBR
在 SSH 中分别输入：

```php
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```

###### 确认安装
分别输入：

```php
sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control
```
这两句命令的输出结果分别为：

```php
sysctl net.ipv4.tcp_congestion_controlnet.ipv4.tcp_available_congestion_control = bbr cubic reno
net.ipv4.tcp_congestion_control = bbr
```

即等号后输出的结果都有 `bbr` 则说明开启成功。

大功告成！

#### 邀请链接
如果您通过[这个邀请链接](https://www.linode.com/?r=94b59a4c8e42daff9649bde1f60abee7f435a108 "这个邀请链接")注册，或填写这个邀请码（referral code）：
`94b59a4c8e42daff9649bde1f60abee7f435a108`
~~你啥也得不到，但我可以从中受益~~

**Enjoy!**
