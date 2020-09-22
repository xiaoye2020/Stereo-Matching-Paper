# Computing the Stereo Matching Cost with a Convolutional Neural Network

## Abstract

相对于传统方法，这里训练了一个神经网络来求匹配代价。神经网络可以衡量两个校正后的匹配块间的相似程度。

## 1.Introduction

介绍了下传统的方法。

本文贡献：1.将神经网络运用到了立体匹配中。

2.当时的效果很好。

## 2.Related work

~

## 3.Computing the matching cost

介绍了下传统方法AD的原理，就是以灰度值大小来衡量两个匹配区域的相似程度。所以作者想到了用监督学习的方式来训练一个神经网络，用来估计两个区域的匹配程度。

### 3.1.Creating the dataset

左图中的以`p(x,y)`为中心的nxn的匹配快，在右图中找一个positive一个negative的匹配块。他们的中心分别为：$q_{neg}=(x-d+o_{neg},y)$其中$o_{neg}$是从集合$\{-N_{hi},...,-N_{lo},N_{lo},...,N_{hi}\}$中任意选出的；$q_{pos}=(x-d+o_{pos},y)$其中$o_{pos}$是从集合$\{-P_{hi},...,P_{hi}\}$中任意选出的。

d是已知视差，$N_{hi} N_{lo} P_{hi}$和匹配快的边长n都是超参数。

### 3.2 Network architecture

<div align=center>
<img src="Images/0501.png">
</div>

总共有L1到L8共8层网络，其中第一层是卷积神经网络，其他的都是全连接层。每一层的卷积核大小跟全连接层的大小由上图所示。其中第八层结果作softmax函数的输入，生成在好匹配和坏匹配的上的分布。前三层左右的权重是一样的；除了最后一层，其他层中间都有ReLU激活函数。如果想改成处理RGB图像的话，卷积核变为三通道就行了。

### 3.3.Matching cost

匹配代价$C_{cnn}(p,d)$是网络的输出：

<div align=center>
<img src="https://latex.codecogs.com/gif.latex?%5Cbg_white%20C_%7Bcnn%7D%28p%2Cd%29%3Df_%7Bneg%7D%28%3CP_%7B9%5Ctimes%209%7D%5E%7BL%7D%28p%29%2CP_%7B9%5Ctimes%209%7D%5E%7BR%7D%28pd%29%3E%29">
</div>

可以比较节省时间的做法：

1.对于一个位置p前三层只用前向传递一次就行了。

2.通过向网络提供全分辨率图像进行传递而非9 × 9 图像块，可以计算出一次正向传播中所有位置的L3层输出。为了实现这一点，我们将L2层和L3层卷积，在L2层中使用尺寸为5×5×32的滤波器，在L3层中使用尺寸为1 × 1 × 200的滤波器，两者都输出200个特征映射。

3.同样，可以使用尺寸为1 × 1 的滤波器替换L4到L8，以计算单个正向传播中所有位置的输出。

## 4.Stereo method

### 4.1. Cross-based cost aggregation
