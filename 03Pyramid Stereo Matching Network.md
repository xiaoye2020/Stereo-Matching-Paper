# Pyramid Stereo Matching Network

## Abstract

以前的很多网络都是利用Siamese networks，来判断两个匹配块的相似度。有些illposed regions就得不到很好的处理。这篇文章提出的网络，充分利用illposed regions周围的context，对这些区域进行较好的重建。

## 1.Introduction
