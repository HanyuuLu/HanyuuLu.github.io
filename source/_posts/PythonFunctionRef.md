---
title: Python function reference
date: 2018-10-01 00:00:00
tags: Python
---

## Sys
> ### sys模块包含了与python解释器和它的环境有关的函数，这个你可以通过dir(sys)来查看他里面的方法和成员属性。
* ```sys.path()  ```  
查询系统环境变量，返回一个list。

* ```sys.path.append('address')```  
添加定义的搜索目录到环境变量。
>作用域仅在该脚本运行时，运行完毕即失效

* ```sys.path.insert(0,'address')```  
同上，不过是插入。
>如果您想要添加永久路径  
>1.将py文件置于环境变量目录下(注意路径顺序和文件名冲突)  
>2.[尚未验证对python3是否可用]在 /usr/lib/python2.6/site-packages 下面新建一个.pth 文件(以pth作为后缀名)将模块的路径写进去，一行一个路径，如```vim pythonmodule.pth```  
>3.使用PYTHONPATH环境变量```export PYTHONPATH=$PYTHONPATH:/home/liu/shell/config```