title: 如何安装黑苹果
date: 2014-04-19 19:31:44
tags: 
- 黑苹果 
- 系统
- clover
categories: 
- 瞎折腾
toc: true
 
---


## 背景
偶尔逛论坛的时候发现了pcbeta这个之前用来看win7信息的神坛竟然还有个mac版，里面都是介绍如何安装黑苹果的帖子，正好最近对mac特口水，就想先安个osx系统试试，看看好用不。
看了各种帖子，各种baidu，各种google，费了3个晚上终于把win8.1和mac双系统安上了。
<!-- more -->

长这样：
![](/images/14e06ad3f2c166c2733f49b377f45c9a6873effb.jpg)

不过mac系统用着还是挺爽的，竟然还能玩我大炉石，立马想去买个mba的节奏啊。

## 黑苹果安装教程－－基础知识介绍
一开始看教程各种专业术语，搞的云里雾里的，这里搞在一起总结一下：

* 分区相关：
1. MBR:
>主引导记录（Master Boot Record，缩写：MBR），又叫做主引导扇区，是计算机开机后访问硬盘时所必须要读取的首个扇区，它在硬盘上的三维地址为（柱面，磁头，扇区）＝（0，0，1）。

2. GPT:
> 全局唯一标识分区表（GUID Partition Table，缩写：GPT）是一个实体硬盘的分区结构。它是EFI(可扩展固件接口标准)的一部分，用来替代BIOS中的主引导记录分区表。但因为MBR分区表不支持容量大于2.2TB（2.2 × 1012字节）的分区，所以也有一些BIOS系统为了支持大容量硬盘而用GPT分区表取代MBR分区表。

* 引导相关：
1. EFI:
> 可扩展固件接口（英文名Extensible Firmware Interface 或EFI）是一种个人电脑系统规格，用来定义操作系统与系统韧体之间的软件界面，为替代BIOS的升级方案。可扩展固件接口负责加电自检（POST）、连系操作系统以及提供连接操作系统与硬件的接口。
EFI最初由英特尔开发，现时由UEFI论坛来推广与发展。

2. UEFI
> 是由EFI1.10为基础发展起来的，它的所有者已不再是Intel，而是一个称作Unified EFI Form的国际组织，贡献者有Intel，Microsoft，AMI，等几个大厂，属于open source，目前版本为2.1。
一般现在新一些的主板都是UEFI（图形化的BOIS）

3. 变色龙
> 变色龙是Voodee团队开发的一个基于Boot-132的EFI引导软件，主要用来引导Mac OS X苹果系统，最初是简短的引导Linux的代码，经过不断改进，已经可以引导Mac,最新版RC5,750及更高版本的可以引导Mac 10.7 lion，而752、755及1800以后的版本可以引导最新的OS X Mountain Lion.

4. Clover
> 跟上面的变色龙差不多，也是一个引导软件，不过功能好像更强大。


## 黑苹果安装教程－－参考内容
强烈建议先去pcbeta的[OS X Mavericks](http://bbs.pcbeta.com/forum.php?mod=forumdisplay&fid=536&page=1)版看一下，里面各种高大上的教程和帖子，特别是顶置中的两个：
1. [【新手必看】基于 OS X Mavericks 系统 黑苹果新手引导 常见疑难解答 必要知识普及帖](http://bbs.pcbeta.com/viewthread-1494831-1-1.html)
2. [【一切尽在这里】远景 OS X Mavericks 置顶向导集合贴【全新改版 新人必看】更新 V4.0](http://bbs.pcbeta.com/viewthread-1470582-1-1.html)

**安装前必看**
p.s. 我的安装主要参考的[这个帖子](http://bbs.pcbeta.com/viewthread-1443155-1-1.html)


## 黑苹果安装教程－－前期准备
1. 查看硬件是否满足条件[链接](http://bbs.pcbeta.com/viewthread-1494831-1-1.html)
1. 确认安装方式
我的电脑是新主板，支持UFEI，所以选择了GPT+UEFI+CLOVER引导的方式，如需用其他方式可参考上面的链接。
1. 制作安装u盘
我用的[这一个](http://bbs.pcbeta.com/viewthread-1483106-1-1.html)


## 黑苹果安装教程－－安装过程和遇到的问题
1. 刚开始准备工作做的不足，就做了个u盘，然后看到一个帖子说可以从mbr直接转到gpt分区（之前装的win7默认是mbr的），然后就手贱用Disk Genius转了一下分区格式，结果电脑立马重启，再也起不来了。。。

2. 于是乎就又买了个u盘，搞了一个win8.1的安装盘，先安装win8。

3. 从抽屉里翻东西偶然翻出一个128g的SSD来，这样电脑硬盘就变成128g SSD装mac，64g SSD装win8，剩下的1t机械硬盘用来放电影电视游戏啥的。

### 硬盘分区
确定了系统安装的方式之后，首先需要确定下用哪个硬盘进行引导，然后进行分区。这里我用64的硬盘作为引导硬盘，因此需要在这硬盘上添加一个EFI的分区用来做引导，这里我申请了一个300m的EFI分区。
> 分区的时候踩到了一个大坑，u盘里到winPE下到DiskGenius好像有问题，分的区一直有问题，在win8安装的时候各种报错，后来用了一个其他的分区工具重新分了一下才安装成功了。这个事情搞了一天，各种搜资料都没解决，抱着试试看都心态重新分了下就好了。所以如果遇到安不上都问题可以试试**重新分区**。

![图片](/images/123.png)

### 安装win8.1
分完区后，安装win8.1就很简单了，[安装u盘](http://bbs.pcbeta.com/viewthread-1448054-1-1.html)全傻瓜式安装。

### 安装mac
这一步是最噁心的，这里需要根据硬件的情况调整各类配置等东西，在这里卡了大概两天左右，后来发现其实是安装u盘里等clover版本太老了。去论坛里搜索帖子[下载](http://sourceforge.net/projects/cloverefiboot/files/)了最新版的clover之后，解压文件并且替换到u盘的EFI/clover文件夹里，顺利完成安装，没有出任何问题。

### 更新驱动
安装完成后，发现进入系统3分钟左右立马死机重启，打开啰嗦模式之后看报错，去论坛搜，发现应该是网卡驱动问题，然后去论坛里搜了一个8111的kext替换了系统驱动后，基本就好了。
还有就是前置的usb3.0接口一插u盘就死机，去论坛搜了一个驱动更新下就好了。

### 安装软件&系统使用
推荐一些资料：
http://www.zhihu.com/question/20386836
https://github.com/hzlzh/Best-App
https://github.com/pubyun/macdev
http://jianshu.io/collection/53eae7e9678f
   
参考：
分区相关：http://dngood.blog.51cto.com/446195/647702/
Chameleon: 来自[百度百科](http://baike.baidu.com/link?url=fn5cGEapxoUig5NHpMe_GlfdT_oeWIvm13lua4qRHFSUKX2oNGum8sJO2h68Grx8pd5UPttFnP20SRFa5c14Fq)
