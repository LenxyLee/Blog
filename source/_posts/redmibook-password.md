---
title: redmibook-password
date: 2023-08-19 19:01:34
tags: 踩坑日志
---

# 红米Redmibook 17 Bios密码找回

只能说这是一个困扰了我很久的问题，之前给bios随手设置了一个密码但是想不起来密码是什么了。所以才有了这篇踩坑日志。

## 提取BIOS

使用H2OUVE对当前运行的bios进行提取

![](../img/image-20230819192523604.png)

## 查找密码

点击Variable 找到SystemSupervisorPW 查看明文密码（对 bios里保存的是明文密码

![](../img/image-20230819193340169.png)
