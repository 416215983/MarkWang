---
layout: post
title: Content-Preserving Image Stitching with Piecewise Rectangular Boundary Constraints
date: 2020-05-15
Author: Mark Wang 
tags: [content-preserving image stitching, panoramic image, rectangling, polygon Boolean operations, piecewise rect- angular boundary]
comments: true
toc: true
---

Content-Preserving Image Stitching with Piecewise Rectangular Boundary Constraints

基于分段矩形边界约束的内容保存图像拼接



预处理

SIFT算法计算图像的特征点，把图像对齐，用LSD直线段检查算法检查图像中所有直线

初始图像拼接

初始化图像的内容保留拼接，APAP算法中对图像进行精准特征对齐，通过全局相似度使缝合更自然和缺少失真

分段矩形拼接

初始图像拼接后，提取每个变形网格的轮廓，并通过多边形布尔并运算获得拼接结果的不规则边界。然后，分析不规则边界上的顶点和交点，以获得规则的边界约束以进行能量优化。为了平衡边界简单性和内容失真，进一步迭代地优化分段矩形边界，通过在规则边界上合并由步长连接的边界段来实现。最后对能量函数进行最小化，并通过变换和融合获得拼接结果。

<img src="https://raw.githubusercontent.com/416215983/MarkWang/master/_posts/images/image-20200521115355613.png" alt="image-20200521115355613" style="zoom:50%;" />
