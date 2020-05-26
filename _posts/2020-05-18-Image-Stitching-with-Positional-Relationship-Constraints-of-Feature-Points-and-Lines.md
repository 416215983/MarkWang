---
layout: post
title: Image Stitching with Positional Relationship Constraints of Feature Points and Lines
date: 2020-05-18
Author: Mark Wang 
tags: [image stitching, panoramas, feature points and lines]
comments: true
toc: true
---



Image Stitching with Positional Relationship Constraints of Feature Points and Lines

基于特征点和线位置关系约束的图像拼接



#### 检测重叠区域

基于模板匹配的重叠区域检测方法

本文基于矩形模板图像的表达方式，当知道左上顶点坐标（或右下顶点）以及矩形的长度和宽度时，其他三个可以确定矩形的顶点

参考图像和目标图像在水平方向上合并以确保它们的公共边界是重合的

<img src="https://raw.githubusercontent.com/416215983/MarkWang/master/_posts/images/image-20200521213226373.png" alt="image-20200521213226373" style="zoom:50%;" />

在目标镜像中选择模板R1的矩形

利用归一化相关法找到参考图像I中与模板图像R1最相似的区域R2。Rr是R2左上角的水平坐标值。

最后确定重叠区的左右边界范围[Rl，Rr]为输出。

#### 重叠区域的双重特征匹配

重叠区域中的关键点由VLFeat库中的SIFT提取和匹配。线特征由线段检测器（LSD）提取。它们在重叠区域通过线点不变式或线结线进行匹配。然后使用RANSAC消除不匹配，并将保留的内在像素视为图像拼接的输入数据。

#### 局部对齐模型

#### 特征点和线的位置关系约束的对齐细化

局部单应性模型已经能够很好地处理视差，该模型仍然存在两个问题。一种是在局部变形之后，在周围像素局部透视变换的共同作用下，线结构可能会弯曲。另一个是有时它会忽略图像的自然性，当将重叠区域的对齐效果外推到非重叠区域时，图像会被拉伸。

在目标图像中，任何点p都可以通过其所在网格的四个顶点V = [v1，v2，v3，v4] T的直线组合来确定。我们使用双线性插值来确定点p的位置：p = wT V，其中插值权重w = [ω1，ω2，ω3，ω4] T并在（Zhang et al。，2016）中计算。因此，图像变换可以看作是网格变换。约束条件的详细信息如下。

![image-20200521221702207](https://raw.githubusercontent.com/416215983/MarkWang/master/_posts/images/image-20200521221702207.png)

点对齐项Ep

线对齐项El

线共线性项Ec

全局相似性项Eg

结果表明，全局相似性约束对于提高图像拼接的自然性起着重要作用，而线共线项约束则在保持线结构方面起着重要作用。
