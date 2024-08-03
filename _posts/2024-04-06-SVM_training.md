---
title: Disscuss on the training data of SVM
date: 2024-04-06 12:00:00 +0000
categories: [Neural_Networks, SVM]
tags: [svm]     # TAG names should always be lowercase
---

# 关于SVM训练数据维度和过程的详细讨论

> 笔者在学习了支持向量机的基本原理后，使用matlab训练时，经常遇到向量维度不一致导致的运算错误，认为其原因在于参考一些文章的做法时，大多数作者并没有细致的解释数据集的排列方式，行向量，列向量等，对于初学者来说有些迷惑。
>
> 本文基于Matlab中的`quadprog` 解函数来分析SVM手写推导过程


## 1. 数据集描述

- $X_{[57\times2000]}$: 训练集样本 ,57行，2000列，每一列是一个样本，每个样本是57维的列向量
- $Y_{2000\times1}$: 训练集标签，2000行，1列，每一行保存一个数（1或-1）描述样本的类别

## 2. 二次型优化问题

#### 2.1拉格朗日求解建立

前面的文章《Support Vector Machines 支持向量机》中提到过（后文简称上篇文章），求解SVM本身就是一个最优化问题，我们由此建立了拉格朗日函数：
$$
L=\frac{1}{2}{||\vec{w}||}^2-\sum\alpha_i[y_i(\vec{w} \cdot \vec{x_i}+b) -1]
$$

$$
L=\sum\alpha_i-\frac{1}{2}\sum_i\sum_j\alpha_i\alpha_jy_iy_j  \vec{x_i}\cdot\vec{x_j}
$$

并且由于我们的样本数据有57维，在二维平面空间并不可分，我们采用了核函数来完成高维映射：
$$
K(\vec{x_i},\vec{x_j})=\phi(\vec{x_i})\cdot\phi(\vec{x_j})
$$
因此改写拉格朗日函数为：
$$
L=\sum\alpha_i-\frac{1}{2}\sum_i\sum_j\alpha_i\alpha_jy_iy_j K(\vec{x_i},\vec{x_j})
$$

#### 2.2 Matlab中的`quadprog` 解函数

`quadprog` 是 Matlab中用于求解二次规划（Quadratic Programming）问题的函数。二次规划问题通常具有以下形式：
$$
\min_x \frac{1}{2} x^T H x + f^T x
$$
其中，$x$是待求解的变量，$H$ 是对称正定矩阵, $f$是一个列向量。

`quadprog` 函数的语法为：

```matlab
[x,fval,exitflag,output,lambda] = quadprog(H,f,A,b,Aeq,beq,lb,ub,x0,options)
```

这里是参数的含义：

- `H`：二次项系数矩阵，通常是对称正定的。
- `f`：一次项系数列向量。
- `A`、`b`：不等式约束矩阵和向量。
- `Aeq`、`beq`：等式约束矩阵和向量。
- `lb`、`ub`：变量下界和上界。
- `x0`：可选的初值。
- `options`：优化选项，可以设置迭代次数等参数。

`quadprog` 函数的返回值包括：

- `x`：最优解。
- `fval`：目标函数的最优值。
- `exitflag`：表示求解器退出状态的标志。
- `output`：包含有关求解器操作的详细信息。
- `lambda`：最优解处的拉格朗日乘子。

使用 `quadprog` 函数可以有效地求解二次规划问题，尤其适用于具有线性和二次约束的优化问题。

#### 2.3 用`quadprog` 求解SVM的拉格朗日乘子

- 需要最大化的值：
  $$
  L=Q(\alpha)=\sum^N_i\alpha_i-\frac{1}{2}\sum_i^N\sum_j^N\alpha_i\alpha_jy_iy_j K(\vec{x_i},\vec{x_j})
  $$

  - 我们的训练集样本是2000个，要处理他们两两之间的内积，$N$是样本个数2000

- 限制条件：

  - $\sum\alpha_iy_i=0$ 见上篇文章中公式5，由$\frac{\partial L}{\partial b}=0$化简得来
  - $0\leq\alpha_i\leq C$ 
    - 如果采用Hard Margin：理论上$C = + \infty$, 在程序中可用较大的数代替（$10^6$）
    - 如果是Soft margin：$C =0.1,0.6,2.1...$

- `quadprog`求的是最小值，我们需要把求$L$最大转换成求$-L$的最小值，这样才能代入求解函数中

$$
\min_x \frac{1}{2} x^T H x + f^T x
$$

$$
min[-Q(\alpha)]=\frac{1}{2}\sum_i^N\sum_j^N\alpha_i\alpha_jy_iy_j K(\vec{x_i},\vec{x_j})-\sum^N_i\alpha_i
$$

- 注意我们的变量是$\alpha$

- 这样对应得到`quadprog` 函数应该传入的参数如下：

  - `H`：一个$N\times N$的矩阵， $N=2000$, 其元素值如下

    - $$
      H_{i,j}=y_iy_jK(x_i,x_j)
      $$

    - `H=Y*Y'.*kernel(X,X);`等价的求H矩阵方式

  - `f`：一个$N\times 1$的列向量， $N=2000$，所有元素值都是$-1$，这样 $f^T$就是一个全是$-1$的行向量，和$\alpha_{[2000\times 1]}$相乘后就是 $-\sum^N_i\alpha_i$ 项

  - `A`、`b`：不等式约束$A\cdot x \leq b$, 未使用，直接赋空矩阵`A=[]`、`b=[]`

  - `Aeq`、`beq`：等式约束$Aeq\cdot x = beq$, 我们要满足$\sum\alpha_iy_i=0$，所以`Aeq=Y'`、`beq=0`, Y训练集标签本身是列向量，转置后`Y'`与$\alpha$列向量乘积=0
  - `lb`、`ub`：变量下界和上界，都是$N\times 1$的列向量，下界所有元素是0，上界都是$C$, 表示所有$\alpha_i$的范围都是$0\leq\alpha_i\leq C$ 
  - 其余参数可选

- 在上篇文章中介绍过的例子，当向量是支持向量，也就是在范围边界上，那么对应拉格朗日乘子$\alpha_i$的值是1，其他的是0，这是一个具体的例子。
- 基于KKT条件：对于支持向量，$\alpha_i \neq 0$ (理论上$>0$)
- 但是由于程序无法达到理论的要求，比如$C\neq+\infty$，存储的0也不是真的0而是很小的值 等等原因，我们需要设置一个判定标准，来判断向量是否是支持向量。也就是：如果 $\alpha_i > threshold$,  那么这个向量是支持向量。
  - 可以选取 $threshold=10^{-4}$，令支持向量的$\alpha_i=1$
  - 同时令$\alpha_i<threshold$ 的其他向量$\alpha_i=0$

至此，我们利用`quadprog` 函数先求解了实际的$\alpha$向量，然后利用判定门限，得到$\alpha$的逻辑值。

## 3. SVM 决策函数参数 $\vec{w}$ 和 $b$ 的求解

#### 3.1 一般情况推导

- 依照上篇文章推导出的公式4，不难发现$\vec{w}$和$\alpha$ 的关系

$$
\vec{w}=\sum_{i=1}^N\alpha_iy_i\vec{x_i}
$$

- 另外，由于支持向量满足$y_i(\vec{w} \cdot \vec{x_i}+b) -1= 0$, 所以

$$
b=\frac{1}{y_s}-\vec{w}^T\cdot \vec{x_s}
$$

- 其中，$w$的维度应该与一个样本$x_i$一致，是$57\times 1$

- 这样得到决策函数：
  $$
  g(x)=w^Tx+b
  $$

#### 3.2 引入核函数 

- 由于我们采用了核函数，已经把$x_i$ 映射到了高维的 $\phi(x_i)$

- 求解的$w$变成了：
  $$
  w=\sum_{i=1}^N\alpha_iy_i\phi(x_i)
  $$
  
- 决策函数变成了：
  $$
  g(x)=w^T\phi(x)+b\\
  =\sum_{i=1}^N\alpha_iy_i\phi^T(x_i)\phi(x)+b\\
  =\sum_{i=1}^N\alpha_iy_iK(x_i,x)+b
  $$

- 预测结果：
  $$
  Y_{pred}=sgn[g(x_{test})]
  $$