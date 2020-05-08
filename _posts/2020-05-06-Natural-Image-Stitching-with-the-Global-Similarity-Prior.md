---
layout: post
title: Natural Image Stitching with the Global Similarity Prior
date: 2020-05-06
Author: Mark Wang 
tags: [image stitching, panoramas, image warping]
comments: true
toc: true
---

Natural Image Stitching with the Global Similarity Prior

基于全局相似先验的自然图像拼接

#### 简介

该方法采用局部扭曲模型，并用网格引导每幅图像的变形。

目标函数用于指定扭曲的所需特征。

除了良好的比齐和最小的局部失真外，我们在目标函数中增加了全局相似性先验。

这一先验限制了每个图像的扭曲，使得它作为一个整体类似于相似性变换。相似变换的选择对结果的自然性至关重要。

我们提出了为每幅图像选择合适的尺度和旋转角度的方法。所有图像的扭曲被一起解决，以使全局失真最小化。

#### 优点

它没有有限视野的问题，这是APAP和SPHP共有的问题

通过一起解决所有图像的扭曲，我们的方法可以最大限度地减少全局失真

它为每个图像指定适当的比例和旋转，以使拼接图像看起来比以前的方法更自然

#### 步骤

我们的方法采用局部扭曲模型，包括以下步骤：

1.特征检测和匹配

2.图像匹配图验证

3.APAP的匹配点生成

4.焦距和3D旋转估计

5.比例和旋转选择

6.网格优化

7.通过纹理映射合成结果

#### APAP的匹配点生成

首先通过SIFT检测每个图像中的特征及其匹配。

在成对对齐的质量方面，APAP表现最佳。因此，对每对相邻图像应用APAP，并使用对齐结果来生成匹配点。

#### 网格变形缝合

拼接方法基于网格的图像变形来缝合图像

![image-20200508115556454](/Users/mark/Desktop/blogSite/_posts/images/image-20200508115556454.png)

对齐项Ψa：通过使匹配点与其对应关系对齐来确保变形后的对齐质量。

局部相似项Ψl：用于正则化，并将对齐约束从重叠区域传播到非重叠区域。

全局相似度Ψg：要求每个变形图像都尽可能进行相似性变换。

![image-20200508115840481](/Users/mark/Desktop/blogSite/_posts/images/image-20200508115840481.png)

权重函数 ![[公式]](https://www.zhihu.com/equation?tex=e_%7Bi%7D%5E%7Bj%7D) 将更多权重分配给远离重叠区域的边缘。 对于重叠区域中的四边形，对齐起着更重要的作用。 另一方面，对于远离重叠区域的边缘，相似性先验更重要，因为没有对齐约束。

![image-20200508120615349](/Users/mark/Desktop/blogSite/_posts/images/image-20200508120615349.png)

网格的最佳变形由以下因素确定：

![image-20200508120730505](/Users/mark/Desktop/blogSite/_posts/images/image-20200508120730505.png)



#### 焦距和3D旋转估计

从两幅图像之间的单应性，我们可以估计两幅图像的焦距。 在执行APAP之后，我们对网格的每个四边形都有一个单应性。 因此，每个四边形给出了对图像焦距的估计

#### 最小线路失真旋转（MLDR）

人类对线条更敏感。因此，我们提出了一种寻找两个相邻图像之间相对于直线对齐的最佳相对旋转的过程。我们首先使用LSD检测器检测线路。通过APAP给出的排列，我们可以找到两个相邻图像Ii和Ij之间的线条的对应关系。每对对应的线唯一地确定相对旋转。我们使用RANSAC作为一个稳健的投票机制来确定II和Ij之间的相对旋转。每条线路的投票权取决于其长度和宽度的乘积。最终的相对旋转为所有内部对象旋转角度的平均值。我们表示φij为由MLDR确定的Ii和Ij之间的相对旋转角。

旋转选择（2D方法）

![image-20200508122308202](/Users/mark/Desktop/blogSite/_posts/images/image-20200508122308202.png)

旋转选择（3D方法）

![image-20200508122329859](/Users/mark/Desktop/blogSite/_posts/images/image-20200508122329859.png)

