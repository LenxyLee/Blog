---
title: Oracle F20 ——古董的SLC加速方案
date: 2025-02-17 23:13:04
tags: 踩坑日志
---

# Oracle F20 ——古董的SLC加速方案

![](../img/Screen-QScvLW6J.png)

## 背景介绍

F20是当初群友在讨论SLC的时候推荐的，由于价格便宜且没有认亲就直接拿下了。这张卡由于是10年前Oracle的存储方案，所以并不是一张纯粹的SLC硬盘，而是一张带四块SLC硬盘的Raid卡。



| Feature                                   | Value            |
|-------------------------------------------|------------------|
| Capacity per card                         | 96 GB (4 x 24 GB)|
| Random 4 K read                           | 100,110 IOPS     |
| Maximum delivered random 4 K write        | 83,996 IOPS      |
| Sequential read (1 M)                     | 1,092 MB/sec     |
| Maximum delivered sequential write (1 M)  | 501 MB/sec       |
| Power consumption (normal running mode)   | 16.5 W           |



![](../img/image-20250917125600767.png)
