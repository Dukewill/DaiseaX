---
title: 一键开启搬瓦工等 OVZ 架构 VPS 的 BBR
date: 2017-08-20 19:47:00
tags: [OVZ, BBR, 搬瓦工]
categories: [教程]
banner: https://dfjvma.dm2301.livefilestore.com/y4mCBemoAc6MPUlLwKndkAgi5IABFHlJiejxJoCH7_cz444J2XEi0Z1ctaND6yZMMnF-8WLLgXA_c0osc2t6uBJQTo2fO5lx_N8ih5aHoDZCYitlnLT-c1G4G4_84isGrjgvWa58aibk3k0RBT66vh3of05qJlUXA5iUGBLBQUeOlEoob4pvsqr5c1Z5mHBNms4OXU58eERajHsmewYqu0McA?width=1280&height=582&cropmode=none
thumbnail:
---
BBR 是一个非常实用的功能，很多人应该都已经用上了，但是长期以来，它一直是 KVM 架构 VPS 的专利，OVZ 这种不完全虚拟化技术从原理上决定了其内核几乎无法修改。不过，总有大神化身为骑士，拯救劳苦大众于水火。<!--more-->

Github 大神 [linhua55](https://github.com/linhua55 "linhua55") 等直接放出了[一键安装脚本](https://github.com/linhua55/lkl_study#one-key-script "一键安装脚本")。为使用 OVZ 架构，主要是使用搬瓦工（Bandwagon）的众人带来了光明。不过这个服务只适用于 64 位 Linux 的 IPv4 网络。

#### 一键安装

登陆 SSH，输入如下脚本

```php
curl https://raw.githubusercontent.com/linhua55/lkl_study/master/get-rinetd.sh | bash
```

最后提示 `XXX started` 即可。

#### 添加监听地址

安装完还没结束，这个脚本中默认只监听 `443` 和 `80` 两个端口，如果你用的是其它端口，请到 `/etc/rinetd-bbr.conf` 添加端口，端口参照文件中的即可

```php
0.0.0.0 443 0.0.0.0 443
0.0.0.0 80 0.0.0.0 80
```

修改完成后，保存，重启 VPS。

#### 验证安装

一般来说，上面的步骤走完以后就没有问题了，如果要确认 BBR 是否已经启用，可以使用 `top` 命令，它会列出 CPU 使用率的情况，其中如果有 `rinetd-bbr` 则表明服务已经启动。

#### 多说一句

我也是新手，如有谬误，欢迎指正。有条件的各位还是尽量使用 KVM 架构，Bandwagon 早已经推出了 KVM 架构的服务器。

**Enjoy！**
