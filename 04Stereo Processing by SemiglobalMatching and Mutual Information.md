# Stereo Processing by SemiglobalMatching and Mutual Information

## Abstract

一种半全局的匹配方法，使用mutual information作为匹配代价。全局的方法一般有个平滑项的约束，SGM从各个方向使用路径优化。另外还有一些后处理，这个算法在当时的精度是很高的，而且运行时间也比较短，在一般测试用的图片上需要1-2秒。

## 1.INTRODUCTION

很多情境中都需要双目重建的技术。现在有两个方面比较重要：一是解决一些难以准确匹配的问题，如：遮挡、重复纹理、光照因素、弱纹理区域等；二是很多场景下会要求实时的重建或者近似实时的重建，速度也是需要考虑的一个方面。

根据Scharstein和Szeliski的调查，大多数的双目立体匹配的方法有四个步骤：代价计算、代价聚合、视差估计与优化、视差修正。

1.代价计算：以前的一些SD，SAD啥的对光照强度比较敏感，这篇文章以MI为代价。

2.代价聚合，是指这个像素点的最终代价值不知是由他自己决定的，还结合了他的邻域内的信息。

3.视差计算：local algorithms中采用winner takes all的策略，选取最小视差。global algorithms一般无代价聚合的步骤，而是定义一个全局的能量函数，其中的平滑项的设置，寻找使全局能量函数最小的策略都不尽相同。全局的算法效果很好但是太慢了，有很高的时间复杂度。

## 2.Semiglobal Matching

### 2.1 Pixelwise Matching Cost Calculation

本文使用互信息（Mutual Information，MI）来度量计算匹配代价。挖坑到时候再填吧，没看懂。

### 2.2 Cost Aggregation

因为容易受到噪声影响，最小的代价并不一定就是正确的结果。这里采用的方法是加了个平滑项进行约束，这个平滑项会惩罚相邻的视差变化。定义能量函数：

<div align=center>
<img src="https://latex.codecogs.com/gif.latex?%5Cbg_white%20E%28D%29%3D%5Csum_%7Bp%7D%28C%28p%2CD_%7Bp%7D%29+%5Csum_%7Bq%5Cepsilon%20N_%7Bp%7D%7DP_%7B1%7DT%5B%7CD_%7Bp%7D-D_%7Bq%7D%7C%3D1%5D+%5Csum_%7Bq%5Cepsilon%20N_%7Bp%7D%7DP_%7B2%7DT%5B%7CD_%7Bp%7D-D_%7Bq%7D%7C%3E1%5D%29">
</div>

公式的第一项是数据项，表示当视差图为D时，所有像素的匹配代价的累加，第二项和第三项是平滑项目，惩罚那些像素p邻域$N_{P}$内的所有像素q，其中第二项是惩罚那些视差差距为1的，力度比较小；第三项惩罚那些视差大于1的，力度比较大。

<div align=center>
<img src="https://latex.codecogs.com/gif.latex?%5Cbg_white%20P_%7B2%7D%3D%5Cfrac%7B%7BP_%7B2%7D%7D%27%7D%7B%5Cleft%20%7C%20I_%7Bbp%7D-I_%7Bbq%7D%20%5Cright%20%7C%7D">
</div>
