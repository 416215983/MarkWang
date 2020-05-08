---
layout: post
title: As-Projective-As-Possible Image Stitching with Moving DLT
date: 2020-04-27
Author: Mark Wang
tags: [MDLT, APAP, Image Stitching]
comments: true
toc: true
---

As-Projective-As-Possible Image Stitching with Moving DLT

基于移动DLT的尽可能投影图像拼接

#### 简介

APAP是图像拼接中图像配准的一种算法，他们将图像分成*C*1×*C*2个单元(100×100)，利用移动直接线性变化法(moving DLT)求解每个单元的单应性矩阵，并逐个投影所有单元。该方法采用局部投影结合全局投影的方式有效地提高了配准精度。之后，他们在原来实验的基础上引入捆绑调整的方法，矫正多幅图像拼接的投影失真

#### 基本思路

1.SIFT得到两幅图像的匹配点对
2.通过RANSAC剔除外点，得到N对内点
3.利用DLT和SVD计算全局单应性
4.将源图划分网格，取网格中心点，计算每个中心点和源图上内点之间的欧式距离和权重
5.将权重放到DLT算法的A矩阵中，构建成新的W*A矩阵，重新SVD分解，自然就得到了当前网格的局部单应性矩阵
6.遍历每个网格，利用局部单应性矩阵映射到全景画布上，就得到了APAP变换后的源图
7.最后就是进行拼接线的加权融合

<img src="/Users/mark/Desktop/blogSite/_posts/images/image-20200505032131820.png" alt="image-20200505032131820" style="zoom: 50%;" />