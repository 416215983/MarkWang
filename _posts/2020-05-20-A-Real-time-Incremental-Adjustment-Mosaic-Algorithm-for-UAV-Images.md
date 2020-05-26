---
layout: post
title: A Real-time Incremental Adjustment Mosaic Algorithm for UAV Images
date: 2020-05-20
Author: Mark Wang 
tags: [Unmanned Aerial Vehicle (UAV);motion recovery structure;incremental bundle adjustment;real-time]
comments: true
toc: true
---

A Real-time Incremental Adjustment Mosaic Algorithm for UAV Images

一种无人机影像实时增量平差镶嵌算法

#### 简介

为实现无人机数据实时处 理，本文提出一种基于 POS辅助的实时增量平差镶嵌计算方法，借 助 初 始 POS 快速建 立影像间拓扑关系，然后对局部重叠影像进行基于CPU-GPU 并行加速 的 SIFT特 征 提 取 与 匹 配 、RANSAC粗差 剔除 ，完成相对定向 ，最 后 使 用 Levenberg-Marquart (列文伯格-马夸尔特法)方法进 行非线性最小二乘优化，在较大搜索范围内求解影像位置姿态的最优解，实现无人机回传影像的实时平差并同步进行纠正处理和虚拟镶嵌成果的展示，满足应急条件下对整个测区的实时观测需求.

####  增量平差算法

SFM的核心思想是通过特征匹配获取多视图影像之间的同名点，然后最小化特征点的重投影误差以求解相机位置、姿态参数和特征点地面坐标的最大似然估计.

是SFM方法的一种，是在特征匹配的基础上选择初始像对得到两幅影像的初始化模型，然后向外扩展，逐渐增加参与平差的影像数量，直到所有影像全部参与平差计算。该算法主要包括影像拓扑关系建立、特征提取与匹配、LM非线性最小二乘迭代计算

<img src="https://raw.githubusercontent.com/416215983/MarkWang/master/_posts/images/image-20200522001900825.png" alt="image-20200522001900825" style="zoom:50%;" />

穷举法即任意两张影像构建匹配像对,借助 初始POS辅助计算新输入影像的足迹多边形，与 已有影像的足迹多边形进行求交运算,当重叠度 超过60%时增加新的影像拓扑关系，进而对新 增加的影像对进行匹配建立精确的拓扑关系，从 而为实时纠正展示节省时间

利用CPU-GPU并行加速 ， 解决SIFT匹配效率较低的问题

使用鲁棒性强的RANSAC方法计算单应矩阵H、基础矩阵F，通过H/F矩阵变换剔 除误匹配点

LM 非线性最小二乘迭代计算,通过 LM 迭代求 解得到影像的位置和姿态参数,接收到新的影像 后重复以上过程，迭代优化所有影像的位置和姿 态参数，直至所有影像添加完毕，所有影像再统 一进行平差解算，得到所有影像的位置和姿态 参数
