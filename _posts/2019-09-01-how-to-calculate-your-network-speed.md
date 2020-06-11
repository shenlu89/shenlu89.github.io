---
layout: post
title: "如何估算文件下载速度"
categories: [Computer]
tags: [cs101, thread, process, concurrency, parallelism]
---

之前做生信的时候，经常要下载各种公开的原始数据，批量的下载十几个GB的数据是经常遇到的。所以在开始下载任务前，需要估算一下当前网络环境下载数据的速度。比如，大约多长时间可以下载完所有的数据。虽然现在的下载工具都有下载进度条和预计完成时间的提示。但从一开始做到心中有数，还是很重要的。

## bit(比特)和Byte(字节)

在开始估算之前，先要搞清楚两个重要的概念，Bit(比特)和Byte(字节)。

在计算机科学中，比特(bit，简称b)是表示信息的最小单位，叫做二进制位，用0或1表示。但比特(bit)的信息量太小，很少直接描述数据大小。一般以字节(Byte)为单位描述信息量。当今的标准是8个bit组成一个字节(Byte，简称B)，即1B = 8b。比如，在UTF-8编码规则下，表示一个英文字符需要一个字节，中文则需要三个字节。

>下载软件的时候经常会看到下载32位(32bit)和64位(64bit)的选择，32bit和64bit代表了计算机CPU一次能够处理的最大位数（通常寄存器和CPU的处理效率是匹配的，但也不一定）。

如下图所示，1B = 2<sup>3</sup>。之后都是以Byte为基本单位表示数据量。1 KB 并不是一千字节(1000KB)，因为计算机只认识二进制，所以在这里的KB是2<sup>10</sup>字节(Byte)，也就是1024个字节(Byte)。1KB = 2<sup>10</sup>Byte，1MB = 2<sup>10</sup>KB = 2<sup>20</sup>B， 1GB = 2<sup>10</sup>MB = 2<sup>30</sup>KB... 不难看出，更高的Byte量级就上一个Byte量级的2<sup>10</sup>(1024)倍。

![](./assets/images/bitByte.jpeg)

## 带宽和网速

网络带宽(Network Bandwidth)，简称带宽，是指在单位时间内能传输的最大数据量，基本单位为bits per second，简写为bps。网络和高速公路类似，带宽越大，就类似高速公路的车道越多，其通行能力越强。网络带宽作为衡量网络特征的一个重要指标。



假如你单位已经安装了宽带业务，或小区宽带已经连到你家，你准备下载一个程序、一个网页或一部电影。也许你认为正在使用服务商声称的全部带宽，其实不然，这就不得不涉及到另一个概念——吞吐量。吞吐量是指在规定时间、空间及数据在网络中所走的路径（网络路径）的前提下，下载文件时实际获得的带宽值。由于多方面的原因，实际上吞吐量往往比传输介质所标称的最大带宽小得多。

**参考文献：**

- [1] [网络带宽](https://baike.baidu.com/item/%E7%BD%91%E7%BB%9C%E5%B8%A6%E5%AE%BD){:target="_blank"}