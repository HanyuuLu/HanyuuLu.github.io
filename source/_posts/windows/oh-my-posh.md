---
title: 提升Powershell使用体验
date: 2019-07-17 00:00:00
categories: Programming
tags:
    - powershell
    - Fluent Terminal
    - oh-my-posh
---

# 终端（Terminal）推荐

## Windows Terminal

GitHub地址：[https://github.com/microsoft/terminal](https://github.com/microsoft/terminal)

*   主题推荐 GitHub地址：[https://github.com/mbadolato/iTerm2-Color-Schemes/tree/master/windowsterminal](https://github.com/mbadolato/iTerm2-Color-Schemes/tree/master/windowsterminal)
*   笔者自己配的主题：[https://github.com/HanyuuLu/toolbox/tree/master/poshTheme](https://github.com/HanyuuLu/toolbox/tree/master/poshTheme)

## Fluent Terminal

GitHub地址：[https://github.com/felixse/FluentTerminal](https://github.com/felixse/FluentTerminal)

## Cmder

GitHub地址：https://github.com/cmderdev/cmder

# PowerShell 美化

## oh-my-posh

### 安装

* 若您从未更改过powershell的执行权限（或者不知道此物为何物），则需要更改powershell的执行权限以运行脚本
* 需要管理员权限
``` powershell
Set-ExecutionPolicy Bypass
```
* 安装posh-git
``` powershell
Install-Module posh-git -Scope CurrentUser
```
* 安装oh-my-posh本体
``` powershell
Install-Module oh-my-posh -Scope CurrentUser
```
### 启用

#### 手动启动

``` powershell
Import-Module oh-my-posh    # 导入模块
Set-Theme Paradox   # 使用Paradox主题
```
#### 伴随powershell自启

如果$profile指向的文件存在，则powershell启动时会顺带启动$profile指向的脚本，所以我们将上述语句写入该文件中
``` powershell
New-Item $PROFILE -Type File    # 创建文件，若提示文件已存在则可以直接编辑文件
"Import-Module oh-my-posh    # 导入模块" >> $PROFILE
"Set-Theme Paradox   # 使用Paradox主题" >> $PROFILE
```
# 字体更换

## 更纱黑体

常规字体是没有办法完全显示oh-my-posh的powerline字符的，因此我们需要一种好看的powerline字符
* 笔者在此安利Sarasa Gothic / 更纱黑体 / 更紗黑體 / 更紗ゴシック
微软应用商店[下载](https://www.microsoft.com/store/productId/9MW0M424NCZ7)
Github Release[下载](https://github.com/be5invis/Sarasa-Gothic/releases)
## 字体更换简介
建议在以下位置更换使用 Sarasa Term SC 字体（Term为终端字体，SC为简体中文，其他简写请参考应用商店或github readme说明）
* cmd
* powershell
* Fluent Terminal
* Windows Terminal
* vsCode （编辑器和内置终端）
* Visual Studio
* 其他可能使用到终端或者编辑代码的位置