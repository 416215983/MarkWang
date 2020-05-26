---
layout: post
title: Seamless Image Stitching in the Gradient Domain
date: 2020-05-10
Author: Mark Wang 
tags: [image stitching, panoramas, Gradient Domain]
comments: true
toc: true
---

Seamless Image Stitching in the Gradient Domain

梯度域中的图像无缝拼接

#### 简介

提出几个估值函数来满足这些要求，并将拼接图像定义为最优。在梯度域中测量接缝区域的缝合质量，拼接图像包含最少量的拼接伪像，即接缝不应引入不会出现在I1或I2中的新边缘。由于图像不相似，将拼接图像I的梯度与I1，I2的梯度进行比较。这减少了由拼接图像之间的整体不一致引起的影响。我们称我们的框架为GIST：梯度域图像拼接

#### GIST1:通过图像导数优化估值函数

第一种方法GIST1通过最小化估值函数Ep来计算拼接图像，Ep是拼接图像的导数与输入图像的导数之间的差异度量。

具体地，令I1，I2为两个对齐的输入图像。设τ1（τ2分别）是图像I1（I2分别）中各自的区域，设ω为重叠区域，如图，其中τ1 ∩ τ2 = τ1 ∩ ω = τ2 ∩ ω = ∅。令W为加权遮罩图像。

<img src="/Users/mark/Desktop/blogSite/_posts/images/image-20200521232854024.png" alt="image-20200521232854024" style="zoom:50%;" />

将GIST1的拼接结果I定义为Ep相对于I^的最小值:

![image-20200522000425551](https://raw.githubusercontent.com/416215983/MarkWang/master/_posts/images/image-20200522000425551.png)

其中U是均匀图像，而dp（J1，J2，φ，W）是J1，J2之间的距离在φ上：

![image-20200522000509748](/Users/mark/Library/Application Support/typora-user-images/image-20200522000509748.png)

∥·∥p表示lp范数。

在I1和I2都具有低梯度的图像位置中，Ep会为拼接图像中的高梯度值造成损失。此属性可用于消除错误的缝合边缘。

#### GIST2：拼接导数图像

一种更简单的方法是缝合输入图像的导数形式：

1.计算输入图像的导数∂I1/∂x,∂I1/∂y,∂I2/∂x,∂I2/∂y

2.拼接导数图像以形成F = (Fx,Fy)，Fx通过拼接∂I1/∂x和∂I2/∂x，Fy通过拼接∂I1/∂y和∂I2/∂y

3.查找梯度最接近F的拼接图像。这等效于最小化dp（∇I，F，π，U），其中π是整个图像区域，U是均匀图像。

