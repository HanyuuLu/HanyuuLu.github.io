---
title: 清除windows更新缓存
date: 2018-10-20 00:00:00
tags: techlog
---
我们将手动清除Windows Update缓存以修复Windows 10上的下载问题。在清除缓存之前，您需要停止Windows更新服务。 为此，请搜索“服务”并以管理员身份打开它。 找到“Windows Update”服务，右键单击它，然后选择“停止”选项。

要清除缓存，请执行以下操作：

1.  按“Win + R”，进入C:\WINDOWS\SoftwareDistribution\并按下Enter按钮。

2.  此文件夹包含与Windows更新相关的所有文件。

3.  选择所有文件并删除所有文件。

您需要重新启动Windows Update。 为此，请再次打开服务并启动Windows Update服务。 要启动该服务，请右键单击该服务并在上下文菜单中选择开始。

要安装最新更新，请导航到设置 - >更新和安全 - > Windows Update，并检查更新。