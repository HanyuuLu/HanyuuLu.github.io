---
title: 算法分析与设计
date: 2019-04-24 23:03:06
cover: https://raw.githubusercontent.com/HanyuuFurude/TechBlog/master/res/rm.png
tags: 
	- os
	- review
categories:
	- review
---
[TOC]
-  课程内容

| 算法高级理论 | NP完全性理论与近似性算法                     |
| ------------ | -------------------------------------------- |
| 高级算法     | 随机化算法、线性规划与网络流                 |
| 基础算法     | 递归分治、动态规划、贪心算法、回溯与分支限界 |
| 算法基础理论 | 算法分析与问题的复杂性                       |

# Chapter 1 概述

-   渐近分析记号
    -   渐近上界$O$
    -   渐进下界$\Omega$
    -   紧渐进界$\Theta$
    -   非紧上界$o$
    -   非紧下界$\omega$
-   O的运算性质
-   NP完全性理论
    -   Easy problem
        -   存在多项式时间算法的问题
    -   Hard problem
        -   需要指数时间算法解决的问题
    -   不可解问题
    -   P&NP
    -   判定问题
    -   k确定性算法和P类问题
        -   对于某个判定问题Π，存在一个非负整数k，对于输入规模为n的实例，能够以$O(n^k)$的时间运行一个确定性算法，能够判定的问题
    -   非确定性算法和NP类问题
        -   对于某个问题Π，存在一个非负整数k，对于输入规模为n的实例，能够以$O(n^k)$的时间运行一个非确定性算法，能够判定的问题
    -   NP完全问题(NPC问题)
        -   令Π是个判定问题，如果问题Π属于NP问题，并且对NP类问题中的每一个问题Π'，都有$\Pi'\propto_p\Pi$，则称问题为NPC问题
    -   NP难问题
        -   令Π是个判定问题，如果对于NP类问题中的每一个问题Π'，都有$\Pi'\propto_p\Pi$，则称问题为NP难问题

# Chapter 2

## 递归与分治策略

-   递归
    -   双递归函数
    -   整数划分问题
    -   Hanoi问题
    -   迭代法求解
        -   正确性-归纳验证
        -   数学归纳法
    -   优缺点

-   分治
    -   特征
        -   问题规模缩小到一定程度可以容易地解决

        -   最优子结构性质<small>该问题可以分解为若干个规模较小的相同问题</small>

        -   利用该问题分解出的子问题的解可以合并为该问题的解

        -   子问题相互独立<small>子问题不包含公共子问题</small>

            >   平衡子问题

            -   二分搜索
            -   大整数乘法

        -   还原迭代法

        -   公式法
            $$
            T(n)=
            \begin{equation}
            \left\{
                \begin{array}{lr}
                O(1)\  n = 1 \\
                aT(\frac{n}{b})+f(n) \ n>1
                \end{array}
            \right.
            \end{equation}
            $$
            基于还原迭代法，可以得到递归方程的解
            $$
            T(n)=n^{log_ba}+\sum_{i=0}^{log_b{n-1}}a^if(\frac{n}{b^i})
            $$
            -   复杂度分析

        -   递归树法

        -   主定理

            -   设$a\geq1$,$b>1$是常数，$f(n)$为函数，$T(n)$ 为非负数，且$T(n)=aT(\frac{n}{b})+f(n)$，则
                1.  若$f(n)=O(n^{log_ba-\epsilon}))$,存在$\epsilon>0$是常数，则有$T(n)=\Theta(n^{log_ba})$
                2.  若$f(n)=O(n^{log_ba}))$，则有$T(n)=\Theta(n^{log_ba}log\ n)$
                3.  若$f(n)=O(n^{log_ba+\epsilon}))$，存在$\epsilon>0$是常数，**且对所有充分大的n有$af(\frac{n}{b})\leq cf(n)$**，c<1是常数，则有$T(n)=\Theta(f(n))$

        -   Strassen矩阵乘法

        -   二分归并排序/合并排序

        -   快速排序

        -   最接近点对问题

# Chapter 3

## 动态规划

-   最优化问题

-   最优性原理
    -   每一阶段的决策仅依赖前一阶段产生的状态
    -   最优子结构性质
    
-   解空间、约束条件、可行解、目标函数、最优解、最优化问题

-   重叠子问题

-   无后效

-   找零问题

-   使用表记录所有已经解决的子问题

-   最短路径问题

-   矩阵连乘问题

	```
	//3d1-1 重叠子问题的递归最优解
	//A1 30*35 A2 35*15 A3 15*5 A4 5*10 A5 10*20 A6 20*25
	//p[0-6]={30,35,15,5,10,20,25}
	#include "stdafx.h"
	#include <iostream> 
	using namespace std; 
	 
	const int L = 7;
	 
	int RecurMatrixChain(int i,int j,int **s,int *p);//递归求最优解
	void Traceback(int i,int j,int **s);//构造最优解
	
	int main()
	{
		int p[L]={30,35,15,5,10,20,25};
	    int **s = new int *[L];
		for(int i=0;i<L;i++)  
	    {  
			s[i] = new int[L];  
	    } 
	
		cout<<"矩阵的最少计算次数为："<<RecurMatrixChain(1,6,s,p)<<endl;
		cout<<"矩阵最优计算次序为："<<endl;
		Traceback(1,6,s);
		return 0;
	}
	
	int RecurMatrixChain(int i,int j,int **s,int *p)
	{
		if(i==j) return 0;
		int u = RecurMatrixChain(i,i,s,p)+RecurMatrixChain(i+1,j,s,p)+p[i-1]*p[i]*p[j];
		s[i][j] = i;
	
		for(int k=i+1; k<j; k++)
		{
			int t = RecurMatrixChain(i,k,s,p) + RecurMatrixChain(k+1,j,s,p) + p[i-1]*p[k]*p[j];
			if(t<u)
			{
				u=t;
				s[i][j]=k;
			}
		}
		return u;
	}
	
	void Traceback(int i,int j,int **s)
	{
		if(i==j) return;
		Traceback(i,s[i][j],s);
		Traceback(s[i][j]+1,j,s);
		cout<<"Multiply A"<<i<<","<<s[i][j];
		cout<<" and A"<<(s[i][j]+1)<<","<<j<<endl;
	}
	
	```

	

``` c++

//3d1-2 矩阵连乘 备忘录递归实现
//A1 30*35 A2 35*15 A3 15*5 A4 5*10 A5 10*20 A6 20*25
//p[0-6]={30,35,15,5,10,20,25}
#include "stdafx.h"
#include <iostream>
using namespace std;
const int L = 7;
int LookupChain(int i, int j, int **m, int **s, int *p);
int MemoizedMatrixChain(int n, int **m, int **s, int *p);
void Traceback(int i, int j, int **s); //构造最优解
int main()
{
	int p[L] = {30, 35, 15, 5, 10, 20, 25};
	int **s = new int *[L];
	int **m = new int *[L];
	for (int i = 0; i < L; i++)
	{
		s[i] = new int[L];
		m[i] = new int[L];
	}
	cout << "矩阵的最少计算次数为：" << MemoizedMatrixChain(6, m, s, p) << endl;
	cout << "矩阵最优计算次序为：" << endl;
	Traceback(1, 6, s);
	return 0;
}
int MemoizedMatrixChain(int n, int **m, int **s, int *p)
{
	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= n; j++)
		{
			m[i][j] = 0;
		}
	}
	return LookupChain(1, n, m, s, p);
}
int LookupChain(int i, int j, int **m, int **s, int *p)
{
	if (m[i][j] > 0)
	{
		return m[i][j];
	}
	if (i == j)
	{
		return 0;
	}
	int u = LookupChain(i, i, m, s, p) + LookupChain(i + 1, j, m, s, p) + p[i - 1] * p[i] * p[j];
	s[i][j] = i;
	for (int k = i + 1; k < j; k++)
	{
		int t = LookupChain(i, k, m, s, p) + LookupChain(k + 1, j, m, s, p) + p[i - 1] * p[k] * p[j];
		if (t < u)
		{
			u = t;
			s[i][j] = k;
		}
	}
	m[i][j] = u;
	return u;
}
void Traceback(int i, int j, int **s)
{

	if (i == j)
		return;
	Traceback(i, s[i][j], s);
	Traceback(s[i][j] + 1, j, s);
	cout << "Multiply A" << i << "," << s[i][j];
	cout << " and A" << (s[i][j] + 1) << "," << j << endl;
}

```

- 最长公共子序列
- 背包问题

``` c++
#include<iostream>
#include<cstring>
using namespace std;

int main()
{
    int N;//物品个数 5
    int V;//背包容量 15
    cin >> N >> V;
    int weight[N + 1];// 5 4 7 2 6
    int value[N + 1];// 12 3 10 3 6
    weight[0] = value[0] = 0;
    int maxTotalValue[V + 1]; //maxTotalValue[i]:背包已装容量为i时,背包里所装物品的最大总价值
    memset(maxTotalValue, 0, sizeof(maxTotalValue));
    for(int i = 1; i <= N; i++)
        cin >> weight[i] >> value[i];
    for(int j = 1; j <= N; j++)
        for(int i = 0; i <= V; i++)
            {
                cout << "背包已装容量:" << i << endl;
                if(i >= weight[j])
                    {
                        if(maxTotalValue[i] >= value[j] + maxTotalValue[i - weight[j]])
                            //cout << "背包中未装入物品:" << j << endl;
                            ;
                        else
                            {
                                maxTotalValue[i] = maxTotalValue[i - weight[j]] + value[j];
                                cout << "背包中已装入物品:" << j << endl;
                            }
                    }
                else
                    //cout << "背包中未装入物品:" << j << endl;
                    ;
            }
    cout << "背包能装物品的最大总价值:" << maxTotalValue[V] << endl;
    return 0;
}
```

# Chapter 4

## 贪心算法

- 局部最优选择
- \*近似解
- 找零问题
- 最优子结构性质
- 贪心选择性质
- 活动选择问题
- 最优前缀码问题
- 最小生成树
	- Prim算法
	- Kruskal算法
- 单源最短路径问题

# Chapter 5

## 回溯法

- 问题的解向量
- 深度优先、广度优先
- 多米诺性质
- 图着色问题
- 搜索树
- 剪枝
- 会场分配问题
- 解空间
	- M叉树、子集树、排列树

- 回溯
	- 递归回溯
	- 迭代回溯

# Chapter 6 

## 分支限界法

- 广度优先、最小耗费优先
- 队列式分支限界法
- 优先队列式分支限界法
- 栈式分支限界法
- 估值
- 代价函数
- 最短路径问题
	- Dijakstra算法
- 最大团问题

# Chapter 7

## 随机化算法

-   伪随机数

-   随机数值算法

    -   主要用于数值问题求解
    -   算法的输出往往是近似解
    -   近似解的精确度与算法执行时间成正比

-   Sherwood算法

  $$\overline{t_a}(n)=\sum_{x\in X_n}\frac{t_A(x)}{|X_n|}$$
  
  -   主打平均性能
  
-   Las Vegas算法

    -   随机算法运行一次得到正确解或者无解
    -   可反复运行LV算法，直到得到正确解为止

-   Monte Carlo算法

    -   设p是一个实数，且$\frac{1}{2}<p<1$。如果一个MC算法对于问题的任一实例得到正确解的概率不小于p，则称该MC算法是p正确的，且称$p-\frac{1}{2}$是该算法的优势。

    -   如果对于同一个实例，Monte Carlo算法不会给出两个不同的正确解答，则称该Monte Carlo算法是一致的

    -   如果重复运行一个一致的p正确的MC算法，每次都进行随机选择，即可产生不正确解的概率→任意小

    -   对于一个一致的p正确MC算法，要提高正确解的概率，只要执行该算法若干次，并选择出现频次最高的解即可。

    -   如果重复调用一个一致的$(\frac{1}{2}+\epsilon)$正确的MC算法2m-1次，得到正确解的概率至少为$1-\delta$,其中

      

$$\delta=\frac{1}{2}-\epsilon\sum_{i=0}^{m-1}
\begin{pmatrix}2i\\i\end{pmatrix}
(\frac{1}{4}-\epsilon^2)^i\leq\frac{(1-4\epsilon^2)^m}{4\epsilon\sqrt{\pi m}}$$

-  主元素问题
-  素数测试