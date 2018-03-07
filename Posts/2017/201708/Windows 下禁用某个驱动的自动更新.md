---
title: Windows 下禁用某个驱动的自动更新
date: 2017-09-04 16:20:00
tags: [驱动, 禁用更新, Windows10]
categories: [教程]
banner: https://dfjvma.dm2301.livefilestore.com/y4mCBemoAc6MPUlLwKndkAgi5IABFHlJiejxJoCH7_cz444J2XEi0Z1ctaND6yZMMnF-8WLLgXA_c0osc2t6uBJQTo2fO5lx_N8ih5aHoDZCYitlnLT-c1G4G4_84isGrjgvWa58aibk3k0RBT66vh3of05qJlUXA5iUGBLBQUeOlEoob4pvsqr5c1Z5mHBNms4OXU58eERajHsmewYqu0McA?width=1280&height=582&cropmode=none
thumbnail:
---
> 提醒：对 Window 10 来说，只适用于专业版和企业版，因为家庭版没有组策略编辑器。

自己的台式机还是 12 年靠兼职攒钱攒下来的，显卡用的是一代神卡 [GTX 460 v2](https://www.geforce.com/hardware/desktop-gpus/geforce-gtx-460/specifications "GTX 460 v2")，到现在依旧坚挺，连守望~~屁股~~先锋这种游戏，开开中低特效都足以应对。但不知道是体质问题还是怎么，这个显卡在某个版本的驱动更新以后，就会在游戏一段时间后出现掉驱动然后假死的情况，平时看视频之类却没有任何异常。<!--more-->

而显卡驱动在 Window 10 下是自动更新的！难道我每次玩游戏都要先装一遍旧版本的显卡驱动？

就在这时，一位在华硕工作的朋友送给我一张 1050 Ti，彻底解决了我的问题，哈哈哈 ...

如果你没有这样一位朋友又没法换硬件的话，最好的方法就是禁用显卡驱动的自动更新了。

#### 禁用更新

这个是整体禁用，步骤非常的简单。

```php
按 Win+R 后输入 gpedit.msc，打开；
打开组策略，找到计算机配置 - 管理模板 - 系统 - 设备安装 - 设备安装限制；
查看右侧的设备安装限制，选择“禁止安装未由其他策略设置描述的设备”；
右键打开编辑属性；
选择禁止安装未由其他设置描述的设备中的“已启用”，然后确认。
```

#### 禁用某个设备的更新

如果只想禁用单个设备驱动程序的更新可以先按照上面的步骤到`设备安装限制`一步。

接着

```php
找到“阻止使用与下列设备安装程序类相匹配的驱动程序安装设备”；
在设备管理器里查看设备属性，点击“详细信息”标签，在属性中找到“类 Guid”，复制下方的数值；
将复制的内容粘贴到上面的策略中
确认即可。
```

通过这种方法，不仅仅是自动更新，手动安装也是被禁止的。希望能帮到恰好需要的人。

**Enjoy！**
