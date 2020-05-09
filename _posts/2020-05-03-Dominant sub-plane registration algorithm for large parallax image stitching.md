---
layout: post
title: Dominant sub-plane registration algorithm for large parallax image stitching
date: 2020-05-03
Author: Mark Wang 
tags: [image stitching, image registration, large parallax image, clustering, dominant sub-plane]
comments: true
toc: true
---

Dominant sub-plane registration algorithm for large parallax image stitching

针对大视差图像拼接的显性子平面配准

#### 简介

本文算法以特征点分布为依据，通过聚类算法实现子平面分割，进而对子平面进行局部配准。

首先，使用__层次聚类__算法对已匹配的特征点聚类，通过一种本文设计的拼接误差确定分组数目，并以各组特征点的聚类中心为新的聚类中心对重叠区域再聚类，分割出目标图像的显性子平面。

然后，求解每个显性子平面的投影参数，并采用就近原则分配非重叠区域的单应性矩阵。

#### 层次聚类确定特征点分组

层次聚类算法是通过计算不同类别数据点间的相似度来创建一棵有层次的嵌套聚类树，具有先聚类后分组的特点。在搜索最优分组数目时，相比于k-means算法无需重复聚类，能节省大量计算，有利于实现自动配准。

Average Linkage的计算方法是计算两个组合数据点中的每个数据点与其他所有数据点的距离，将所有距离的均值作为两个组合数据点间的距离，其结果比前两种方法更合理。

#### 二次聚类分割重叠区域

本文使用就近原则处理其所含像素点，将其归于距离最近的一类，即以各类中心为 中心，对整个重叠区域再聚类.

#### 图像拼接

![image-20200507162428164](https://raw.githubusercontent.com/416215983/MarkWang/master/_posts/images/image-20200507162428164.png)

配准精度

使用加权融合法融合图像，加权融合法能够更直接地 展示出真实的配准情况。

最近邻插值法对图像投影后的边缘区域进行插值。最近邻插值法跟据相邻像素值确定填充像素值，不会因空缺区域的像素初始值为零而导致新像素值的减小。

