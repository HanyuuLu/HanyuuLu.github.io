---
title: Jupyter和conda协同工作环境搭建和常见错误处理
date: 2019-10-03 00:00:00
tags: 
	- conda
	- jupyter
---
# 避坑安装方法

> 如果您还没有安装jupyter notebook，那么按照如下方法可以让jupyter和conda协同工作

``` bash
# 在base环境下
# 安装jupyter本体
pip install jupyter
# 安装nb_jupyter
pip install nb_conda
# 完成
```

* 好啦，开始使用就完事辣~

# 关于其他方法安装导致故障的原因和修复

## 方法a

* 安装ipykernel：

  ```
  conda install ipykernel
  ```

* 在虚拟环境下创建kernel文件：

  ```
  conda install -n [环境名] ipykernel
  ```

* 激活conda环境： 

  ```
  source activate [环境名]
  ```

* 将环境写入notebook的kernel中
	```
	python -m ipykernel install --user --name[环境名] --display-name "[显示名])"
	```

### Fix

环境配置列表在```%userprofile%\Appdata\Roaming\jupyter\kernels\[环境名]\kernel.json```下，手动修正即可

### 调试思路

conda的工作就是更换环境变量，可以使用```sys.path```查看conda是否正常工作