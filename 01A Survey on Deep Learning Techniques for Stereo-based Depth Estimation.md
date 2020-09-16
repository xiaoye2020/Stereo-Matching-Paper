# A Survey on Deep Learning Techniques for Stereo-based Depth Estimation

## 1.INTRODUCTION

1.第一代方法是匹配图片间的像素，这些图片是由精确标定好的相机拍摄的。这种方法对弱纹理，遮挡，重复纹理等区域的处理效果不好。

2.人类看一个物体除了视觉上真正看到的，还有一些先验知识来辅助感知这个物体。第二代方法试图将问题转化为学习任务来利用这些先验知识。

3.第三代方法是利用了深度学习技术及大型数据集。

## 2.SCOPE AND TAXONOMY

文章只讨论双目立体匹配和多目立体匹配（Multi-View Stereo），对于单目的重建和基于视频的重建并不在这篇综述的讨论范围内。

---

重建的方法分为了两类：

第一类是模拟传统的立体匹配方法显示地学习，包括特征提取、特征匹配和代价聚合、视差估计这这步骤，分别独立来做。

第二类是端到端的网络直接获取视差图。早期的方法把视差估计视为一个回归问题，这种方法快速简单但是需要大量数据集的支撑。第二种是根据传统方法的步骤，把端到端的网络分为几个阶段，每个阶段分别解决不同的问题。

## 3.DATASETS

<img align="center" src="Images/0101.png">

## 4.DEPTH BY STEREO MATCHING

一般的，双目重建的核心思路是使下面的能量函数最小，其中第一项为匹配代价；第二项为一个正则项用来施加约束，如平滑、左右一致性约束等。

<div align=center>
<img src="https://latex.codecogs.com/gif.latex?E%28D%29%3D%5Csum_%7Bx%7DC%28x%2Cd_%7Bx%7D%29++%5Csum_%7Bx%7D%5Csum_%7By%5Cin%20N_%7Bx%7D%20%7DE_%7Bs%7D%28d_%7Bx%7D%2Cd_%7By%7D%29" width=50%>
</div>

分为四个步骤,特征提取、特征匹配、视差计算、视差后处理。前两步构造代价空间。第三步正则化代价空间，然后通过最小化能量函数得到初步的视差图。最后一步是对得到的视差图进行优化和后处理。

<img align="center" src="Images/0102.png">

### 4.1Learning feature extraction and matching
