---
title: Python炼丹炉（maching learning）环境搭建常见问题
date: 2019-01-31 00:00:00
tags: Python
---
# Using Anaconda/Conda with Powershell

* 在anaconda/conda中创建虚拟python环境：
  conda create -n env_name python=x.x

* 在anaconda prompt 和 cmd中，激活一个环境：
  activate env_name

* Powershell添加conda/anaconda支持：
  1. 安装库：
      ``` powershell
      conda install -n root -c pscondaenvs pscondaenvs
      ```

    > 如果因为某些原因无法通过上述方案安装……请手动下载安装
    >
    > Offical Link： https://github.com/BCSharp/PSCondaEnvs

  2. 更改PowerShell配置：
      以**管理员**身份启动PowerShell，并执行```Set-ExecutionPolicy RemoteSigned```

    ``` powershell
    PS C:\Hanyuu> Set-ExecutionPolicy RemoteSigned

    执行策略更改
    执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如http://go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies帮助主题所述。是否要更改执行策略?
    [Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助(默认值为“N”):Y
    ```

  3. 此时可以正常使用Powershell切换到特定环境

  ``` powershell
  activate [EnvironmentName]
  ```

  4. 原理：其实……就是……加了个……ps1文件替换bat……（呵呵呵~）


# Using Jupyter with anaconda/conda

*   原理：在虚拟环境下缺少kernel.json文件

*   Add a exists environment into Jupyter

    1.  Install ipykernel

        ```powershell
        conda install ipykernel
        ```
    2.  Create kernel files in virtual environment

        ```powershell
        conda install -n [EnvironmentName] ipykernel
        ```
    3.  activate virtual environment

        ```powershell
        activate [EnvironmentName]
        ```
    4.  Add your virtual environment into kernel for notebook

        ```powershell
        python -m ipykernel install --user --name [EnvironmentName] --display-name [EnvironmentDisplayName]
        ```
    5.  open your jupyter notebook and enjoy your pythoning : )

        ``` powershell
        jupyter notebook
        ```
*   If your environment has not been created...

    ``` powershell
    conda create -n [EnvironmentName] python=?.? ipykernel
    ```
*   Delete a environment

    ``` powershell
    jupyter kernelspec remote [EnvironmentName]
    ```
## Jupyter 自动补全和主题修改
[link](https://www.cnblogs.com/qiuxirufeng/p/9609031.html)