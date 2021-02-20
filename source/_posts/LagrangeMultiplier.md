---
title: lagrange multiplier拉格朗日乘子法
date: 2019-02-13 15:05:58
tags: math, lafrange mutplier
---
基本的拉格朗日乘子法就是求函数f(x1,x2,...)在约束条件g(x1,x2,...)=0下的极值的方法。
其主要思想是将约束条件函数与原函数联立，从而求出使原函数取得极值的各个变量的解。

# 定义
对于具有l个等式约束的n维优化问题
$$min\ f(x_1,x_2,\cdot \cdot \cdot),\ s.t. \  h_k(x_1,x_2,\cdot\cdot\cdot,x_n)\ (k=1,2,\cdot\cdot\cdot,l)$$
把原目标函数$f(x)$改造成为如下形式的新的目标函数
$$F(x,\lambda)=f(x)+\sum_{k=1}^l\lambda_kh_k(x)$$
式中的$h_k(x)$就是原目标函数$f(x)$的等式约束条件，而待定系数$\lambda_k$称为拉格朗日乘子。这种方法称为拉格朗日乘子法。在极值点处，有
$$\frac{\partial F}{\partial \lambda x_i}=0\  (i=1,2,\cdot\cdot\cdot,n)$$
和
$$\frac{\partial F}{\partial \lambda_k}=0\ (k=1,2,\cdot\cdot\cdot,l)$$
，共有$n+l$个方程，足以算出这$n+l$个变量，此法也称为升维法。
# 基本原理
拉格朗日乘子法是一种经典的求解条件极值的解析方法，可将所有约束的优化模型问题转化为无约束极值问题的求解。一般带不等式约束的最优化问题求解如下式：

$$
\begin{equation}
\left\{
	\begin{array}{lr}
	min\  f(x) & \\
	s.t.\  g_i(x)\leq0\  (j=1,2,\cdot\cdot\cdot,J)
	\end{array}
\right.
\end{equation}
$$

拉格朗日乘子法是用于变量无关的是常数$\lambda_i\  (j=1,2,\cdot\cdot\cdot,J)$分别乘各约束函数$g_i(x)\leq0\ (j=1,2,\cdot\cdot\cdot J)$并与目标函数相加得到如下的拉格朗日函数：

$$L(x,\lambda,v)=f(x)+\sum_{j=1}^{J}{\lambda_j[g_j(x)+v^2_j]}$$

，式中：$x=[x_1,x_2,\cdot\cdot\cdot,x_j]^T$为自变量；$\lambda=[\lambda_1,\lambda_2,\cdot\cdot\cdot,\lambda_j]^T$为拉格朗日乘子量；$v=[v_1,v_2,\cdot\cdot\cdot,v_j]^T$为松弛变量。
则$L(x,\lambda ,v)$在$x^*​$处取极值的必要条件为：
$$
\begin{equation}
\left\{
	\begin{array}{lr}
	\frac{\partial L}{\partial x_k}=\frac{\partial f(x)}{\partial x_k}+\sum^J_{j=1}{\lambda_j\frac{\partial g(x)}{\partial x_k}}=0 & \\
	\frac{\partial L}{\partial \lambda}=\sum^J_{j=1}{(g_i(x)+v^2_j)}=0 & \\
	\frac{\partial L}{\partial v}=\sum^J_{j=1}2\lambda_jv_j=0
	\end{array}
\right.
\end{equation}
$$

，依据上式求得$x^*$即为最优解。

>   参考：
>
>   *   [拉格朗日乘子法]( https://baike.baidu.com/item/%E6%8B%89%E6%A0%BC%E6%9C%97%E6%97%A5%E4%B9%98%E5%AD%90%E6%B3%95/1946079)
>   *   [拉格朗日乘子法：写得很通俗的文章](https://blog.csdn.net/ndzzl/article/details/79079561)
>   *   非盈利非商业使用，侵删