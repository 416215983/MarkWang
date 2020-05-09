---
layout: post
title: Adaptive As-Natural-As-Possible Image Stitching
date: 2020-04-30
Author: Mark Wang
tags: [AANAP, Image Stitching]
comments: true
toc: true
---

#### 简介

AANAP算法结合了投影变换、相似变换和仿射变换，能够有效处理拼接过程中的投影失真，使拼接后的图像更加符合视觉审美。实质上，AANAP在配准方面也采用了APAP算法的思路，即利用Moving DLT方法计算局部投影和全局投影。

#### 局部单应性

在重叠中没有偏移的区域使用Moving DLT来预估局部单应性，并且使用单应线性化将其外推到非重叠区域，来减少透视失真，用于外推法的加权方案在参数选择上有较小的依赖

#### 全局相似性变换

获取特征点的匹配后，使用带有阈值εg的RANSAC去除孤立点

然后使用带有阈值εl的RANSAC找出具有最大的内点的平面的单应性，其中εl<εg，并且移除内点

重复此操作，直到内点的数量小于η

每组匹配的内点用于计算单个相似性变换

然后再检查对应于变换的旋转角，并选择具有最小的旋转角

#### 全局相似性变换的整合

在计算全局相似变换后，利用全局相似变换来调整目标图像的变换，以减轻全景图中的透视失真，如果只调整非重叠区域的变换，拼接结果可能会产生不自然的视觉效果。因此，使用以下公式将整个目标图像的局部变换逐步更新为全局相似性变换：

![image-20200507015533842](https://raw.githubusercontent.com/416215983/MarkWang/master/_posts/images/image-20200507015533842.png)

用全局相似性变换更新目标图像的变换会导致先前对准的参考图像和目标图像之间的重叠区域不对准。因此，我们需要通过适当地将改变从目标图像传播到参考图像来补偿改变。现在可以获得参考图像的局部变换为：

![image-20200507015446825](https://raw.githubusercontent.com/416215983/MarkWang/master/_posts/images/image-20200507015446825.png)

