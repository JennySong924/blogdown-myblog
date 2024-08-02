---
title: 8 月 - 论文笔记
author: J Song
date: '2024-08-02'
slug: []
categories:
  - 技术文章
  - 统计

description: 2024-08-02-8
keywords: '2024,08,02,8'
lastmod: '2024-08-02T17:39:23+08:00'
---

## Jiacheng Miao, Assumption-lean and Data-adaptive Post-Prediction Inference

目前有许多研究在建模时，对于 unlabeled Y 先进行预测，例如使用 ML 方法预测 Y 的 label，再使用这些预测出来的 Y，加上原本就有 label 的 Y，一起进行其他分析。前一步预测的准确性可能会给下一步分析结果带来未知影响。文章提出了 POP-Inf estimator，无论第一步的预测模型表现如何，它都可以达到不弱于传统 estimator（即使用原本有 label 的 Y 得到的 estimator）的效果，证明了它的 asymptotic normality 性质，并提供了具体算法。

想法是在 classic estimator 的基础上增加一项 label 和 unlabel part 的相差项：

$$\frac{1}{n}\sum_{i=1}^n \Phi (y_i,x_i;\theta) + \omega \odot \bigl\{-\frac{1}{n}\sum_{i=1}^n \Phi(\hat{f}(z_i),x_i;\theta) + \frac{1}{N}\sum_{i=n+1}^{n+N}\Phi(\hat{f}(z_i),x_i;\theta)\bigr\}=0$$
