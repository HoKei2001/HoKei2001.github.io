---
title: SVM Decision Function Solution
date: 2024-04-07 12:00:00 +0000
categories: [Neural_Networks, SVM]
tags: [svm]     # TAG names should always be lowercase
---

# SVM 决策函数的求解方法

> 上篇文章利用Matlab中的`quadprog`函数求解出拉格朗日算子矩阵$\alpha$
>
> 本文将介绍如何利用$\alpha$求解非线性核SVM最终的决策函数

- 多项式形式的核函数，二次为例：

$$
K(u,v)=(u^Tv+1)^2
$$

- 决策函数：
  $$
  g(x)=w^T\phi(x)+b\\
  =\sum_{i=1}^N\alpha_iy_i\phi^T(x_i)\phi(x)+b\\
  =\sum_{i=1}^N\alpha_iy_iK(x_i,x)+b
  $$

  - $\alpha_i$是我们已经求出的矩阵$\alpha$的元素
  - $y_i$是已知的训练集标签
  - $K(x_i,x)=(x_i^T \cdot x+1)^2$
    - $x_i$是训练集第$i$个样本
    - $x$是我们需要判断的一个样本，其维度与$x_i$一致

- 问题在于如何求解$b$

  - 由于我们已知支持向量的判别结果被定义为±1

  - 对于我们已知的训练集，它包含两类样本，分别是$x_+$和$x_-$
    $$
    \vec{\omega}\cdot\vec{x_+}+b \geq 1 , 对于+样本\\
    \vec{\omega}\cdot\vec{x_-}+b \leq -1, 对于-样本
    $$


  - 刚好在判定边界上的支持向量代入决策函数的结果是已知的
  - 我们可以通过这一点来计算所有支持向量对应的$b$，最后取平均值