---
title: Anaconda导出环境/从外部安装环境
date: 2019-02-25 00:00:00
tags:
    - python
categories:
    - programming
---
> 填坑中...
导出已有环境：
``` py
conda env export > environment.yaml
```
环境会被保存在 environment.yaml文件中。
当我们想再次创建该环境，或根据别人提供的.yaml文件复现环境时，可以：
``` py
conda env create -f environment.yaml1
```