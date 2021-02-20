---
title: 再生核希尔伯特空间(RKHS)
date: 2019-02-17 00:00:00
tags: deep learning, RKHS, math
mathjax: true
---
> **【转载，文末有出处】**
# 核函数

每一个函数$f$都可以看做一个无限维的向量，那么二元函数$K(x,y)$ 就可以看做是一个无限维的矩阵。如果它满足：

* 正定性
$$
\int \int f(\mathrm{x})K(\mathrm{x},\mathrm{y})f(\mathrm{y})d \mathrm{x} d\mathrm{y} \geq 0
$$
* 对称性:
$$
K(\mathrm{x},\mathrm{y}) = K(\mathrm{y},\mathrm{x})
$$
那么它就是一个核函数。

与矩阵特征值和特征向量类似，核函数存在特征值和特征函数（将函数看做无限维向量）。也就是，
$$
\int K(\mathrm{x},\mathrm{y}) \psi (\mathrm{x}) d\mathrm{x} = \lambda \psi (\mathrm{y})
$$
对于不同的特征值 $\lambda_1$, $\lambda_2$及其对应的特征方程 $\psi_1(\mathrm{x})$ , $\psi_2(\mathrm{x})$ ,

$$
\int \lambda_1 \psi_1(\mathrm{x}) \psi_2(\mathrm{x}) d \mathrm{x} = \int \lambda_2 \psi_2(\mathrm{x}) \psi_1(\mathrm{x}) d \mathrm{x}
$$

因此，$<\psi_1, \psi_2> = \int \psi_1(\mathrm{x}) \psi_2(\mathrm{x}) d \mathrm{x} = 0$ 。也就是说特征方程是正交的。

一个核函数对应无穷个特征值 $\{ \lambda_i \}_{i=1}^\infty$ 和无穷个特征方程 $\{ \varphi_i \}_{i=1}^\infty$ 。和矩阵类似，
$$
K(\mathrm{x},\mathrm{y}) = \sum_{i=0}^{\infty} \lambda_i \psi_i(\mathrm{x}) \psi_i(\mathrm{y})
$$
这就是Mercer定理。这里， $<\psi_i, \psi_j>=0 , i \ne j 。 \{ \psi \}_{i=1}^{\infty}$ 是原来函数空间的一组正交基。
RKHS

将 $\{\sqrt{ \lambda_i} \psi_i \}_{i=1}^\infty$ 作为一组正交基构建一个希尔伯特空间 $\mathcal{H}$ 。这个空间中的任何一个函数（向量）都可以表示为这组基的线性组合。如

$f= \sum_{i=1}^{\infty} f_i \sqrt{\lambda_i} \psi_i$

那么 f 就可以表示为 $\mathcal{H}$ 中的一个无限维的向量：
$$
f= (f_1,f_2,...)_{\mathcal{H}}^T
$$
$K(\mathrm{x},\mathrm{y})$ 表示二元函数或无限维矩阵， $K(\mathrm{x},\cdot)$ 就可以表示矩阵第 x 行的一元函数或无限维向量，也就是固定核函数的一个参数为 $\mathrm{x}$ ，那么
$$
K(\mathbf{x},\cdot) = \sum_{i=0}^{\infty} \lambda_i \psi_i (\mathbf{x}) \psi_i
$$
将每一项除去对应的基底，对应到空间 $\mathcal{H}$中的向量就是
$$
K(\mathrm{x},\cdot) = ( \sqrt{\lambda_1} \psi_1(\mathrm{x}), \sqrt{\lambda_2} \psi_2(\mathrm{x}), ... )_{\mathcal{H}}^T
$$
同样的，
$$
K(\mathrm{y},\cdot) = ( \sqrt{\lambda_1} \psi_1(\mathrm{y}), \sqrt{\lambda_2} \psi_2(\mathrm{y}), ... )_{\mathcal{H}}^T
$$
因此，
$$
< K(\mathbf{x},\cdot), K(\mathbf{y},\cdot) >_\mathcal{H} = \sum_{i=0}^{\infty} \lambda_i \psi_i (\mathbf{x}) \psi_i(\mathbf{y}) = K(\mathbf{x},\mathbf{y}) 。
$$
以上就是核的可再生性(reproducing)，即用核函数来再生两个函数的内积。 $\mathcal{H}$ 也被叫做可再生核希尔伯特空间(RKHS, reproducing kernel Hilbert space)。

如果定义了一个映射，
$$
\boldsymbol{\Phi} (\mathbf{x}) = K(\mathbf{x},\cdot) = (\sqrt{\lambda_1} \psi_1 (\mathbf{x}), \sqrt{\lambda_2} \psi_2 (\mathbf{x}), \cdots )^T
$$
将点 $\mathrm{x}$ 映射到空间 $\mathcal{H}$ 。那么，
$$
< \boldsymbol{\Phi} (\mathbf{x}), \boldsymbol{\Phi} (\mathbf{y}) >_\mathcal{H} = < K(\mathbf{x},\cdot), K(\mathbf{y},\cdot) >_\mathcal{H} = K(\mathbf{x},\mathbf{y}) 。
$$
因此，我们并不需要知道这个映射是什么，特征空间在哪里，只要是一个对称正定的函数 K ，就必然存在映射 $\Phi$ 和特征空间 $\mathcal{H}$ ,使得
$$
< \boldsymbol{\Phi} (\mathbf{x}), \boldsymbol{\Phi} (\mathbf{y}) > = K(\mathbf{x},\mathbf{y}) 。
$$
这就是所谓的核技巧(kernel trick)。
> 转载:
> * [再生核希尔伯特空间-cplusplus](https://zhuanlan.zhihu.com/p/29527729) \
> 所有权利归原作者所有，非盈利非商业用途，侵删