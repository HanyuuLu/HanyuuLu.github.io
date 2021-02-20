---
title: .NET Core 应用打包和部署到docker上的一些注意事项
date: 2020/10/28
categories: 
    - programming
tags:
    - .NET Core
    - ASP .NET
    - Docker
---
[toc]

### 环境概要

本文档使用
* .NET Core 3.1.401 win-x64
* Docker Community win-x64
  * Docker Engine 19.03.13
### 打包
#### 自带依赖发布到目标平台

*   例子

```powershell
dotnet publish -r linux-x64 --configuration Release
```

*   说明

    *   publish命令会自动restore依赖
    
    *   configuration分为`Debug`和`Release`，建议使用`Release`发布应用（并配置对应的配置文件）

    *   -r：要编译的目标平台，具体请参阅[.NET Core 运行时标识符 (RID) 目录 | Microsoft Docs](https://docs.microsoft.com/zh-cn/dotnet/core/rid-catalog)

        一些常用的RID如下
    
        >   -   可移植（.NET Core 2.0 或更高版本）
        >
        >       -   `win-x64`
        >       -   `win-x86`
        >       -   `win-arm`
        >       -   `win-arm64`
        >
        >       -   `linux-x64`（大多数桌面发行版，如 CentOS、Debian、Fedora、Ubuntu 及派生版本）
        >       -   `linux-musl-x64`（使用 [musl](https://wiki.musl-libc.org/projects-using-musl.html) 的轻量级发行版，如 Alpine Linux）
        >       -   `linux-arm`（在 ARM 上运行的 Linux 发行版本，如 Raspberry Pi Model 2 及更高版本上的 Raspbian）
        >       -   `linux-arm64`（在 64 位 ARM 上运行的 Linux 发行版本，如 Raspberry Pi Model 3 及更高版本上的 Ubuntu 服务器 64 位）
        >
        >       -   `osx-x64`（最低 OS 版本为 macOS 10.12 Sierra）

#### 打包程序为单文件

添加参数`-p:PublishSingleFile=true`

```powershell
dotnet publish -r linux-x64 --configuration Release -p:PublishSingleFile=true
```

#### 裁剪未使用的依赖

>   从 .NET Core 3.0 开始，SDK 作为预览功能提供。此功能可能会导致问题，如果发现问题请尝试不启用此参数

添加参数`-p:PublishTrimmed=true`

``` powershell
dotnet publish -r linux-x64 --configuration Release -p:PublishSingleFile=true -p:PublishTrimmed=true
```



>   -p 参数也可写在配置文件中，请参阅[dotnet publish 命令 - .NET Core CLI | Microsoft Docs](https://docs.microsoft.com/zh-cn/dotnet/core/tools/dotnet-publish#msbuild)和[用于 ASP.NET Core 应用部署的 Visual Studio 发布配置文件 (.pubxml) | Microsoft Docs](https://docs.microsoft.com/zh-cn/aspnet/core/host-and-deploy/visual-studio-publish-profiles?view=aspnetcore-3.1)

### .NET Core 发布后在Ubuntu基础镜像上运行的常见问题

1.  提示没有icu依赖

    icu是ASP.NET Core在Linux系统上的全球化实现，Docker基础镜像默认无此依赖，需要在打包镜像时先安装依赖，参照[全球化和 ICU | Microsoft Docs](https://docs.microsoft.com/zh-cn/dotnet/standard/globalization-localization/globalization-icu)

2.  提示没有合适版本的libssl

    Ubuntu基础镜像缺省libssl版本对.NET Core 3不兼容

解决方案：参考如下Dockerfile，使用国内镜像安装依赖

``` dockerfile
FROM ubuntu:latest
ADD ./KoroneLibrary/bin/Release/netcoreapp3.1/linux-x64/publish/  /
ADD ./sources.list /etc/apt/sources.list
RUN chmod 744 /KoroneLibrary && apt update && apt upgrade -y && apt install icu-devtools -y && apt install libssl1.1 -y && apt auto-remove -y && apt clean -y
CMD ["/KoroneLibrary"]
VOLUME ["/Lib"]
EXPOSE 5000
```



### 参考文档

*   [dotnet publish 命令 - .NET Core CLI | Microsoft Docs](https://docs.microsoft.com/zh-cn/dotnet/core/tools/dotnet-publish)

