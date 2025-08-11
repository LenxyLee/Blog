---
title: 为什么不是试试看神奇的Intel FM10K
date: 2025-08-07 02:36:54
tags: 灵车日志
---

## 前言

如果说闲言少叙那我总想多说点什么，第一次遇到FM10K是在cyberbus上面。有人搞了一块Intel FM10840的网卡 **Silicom PE3100G2DQIRM**！

![](../img/image-20250807024139021.png)

没错，看起来有点抽象。但这就是Intel的Multi-host Modular Server Platform

这个东西可以充当一个简易的交换机，一张网卡拆分多个端口可以被多台设备使用。

![](../img/image-20250807024253740.png)

由于看着很神奇，所以手痒痒就想去折腾折腾看看这套神奇的东西。

只可惜FM10K已经是EOL的产品了，后续的软件折腾还是比较麻烦的事情。

# 网卡介绍

> 拿到这张卡实属捡漏，感谢商家

这次拿到的卡是Silicom的PE3100G2DQIR，基于Intel FM10420 芯片打造。由于没有这张卡的Block Diagram 所以只能用另一款来代替。

![](../img/image-20250807025548532.png)

很简单的结构，两个QSFP28的口子，可以拆分成4个10/25G。由于没有拆分，所以是一整个PCIE X16 Gen3 但是这张卡让我没想到的是需要外介一个12V的供电。

Intel对FM10K这个控制器的定义以太网与交换机的混合体。实际你完全可以把它看成是一个数据平面，把host当成一个控制屏幕。通过控制平面下发指令操作转发平面完成对数据的转发/丢弃/旁路丢给host。

- 4 MB shared memory 
- L2/L3/L4/OpenFlow forwarding
- 32K 40-bit TCAM entries 
- 16K MAC and NextHop tables 
- NSH Service Function Classifier and Forwarder

这或许就是一个交换机芯片。有TCAM 16K的MAC表 32K的TCAM条目（感觉比某些螃蟹交换机配置都好）

## 食用指南

由于使用的是 Silicom 的方案，可使用其 RDIF控制工具(Silicom Linux RDI Control Utility)。该工具实现了大部分管理功能，可作为后台常驻的 daemon 运行。它通过 CLI方式向管理平面暴露 API，对下通常通过内核驱动（UIO）与数据平面交互。

注：不开启Rdif时，网口无法正常的启用。



