---
title: systemCall添加自定义项目
date: 2019-03-18 18:14:00
tags:
---

>p.s:因为这个md解析器有些调皮……所以可能偷偷改了一些样式导致阅读障碍…… \
>如果遇到了请[帮助我](mailto:Furude_Hanyuu@outlook.com)改正这些错误，谢谢。
## 一，基本信息

| 实验题目 | 向 Linux 内核增加一个系统调用 |
| ---------- | ----------------------------- |
| 完成人姓名 | Hanyuu Furude     |
| 学号  | 你猜( ﹁ ﹁ ) ~→    |
| 报告日期 | 2019/03/18     |

## 二，实验目的

通过实验，熟悉 Linux 操作系统的使用，掌握构建与启动 Linux 内核的方法；
掌握用户程序如何利用系统调用与操作系统内核实现通信的方法，加深对系统调
用机制的理解；进一步掌握如何向操作系统内核增加新的系统调用的方法，以扩 展操作系统的功能。

## 三，实验内容

1. Linux 环境下的 C 或 C++编译和调试工具的使用。

2. 向 Linux 内核增加新的系统调用，系统调用名称和功能自行定义，但必须 实现如下输出功能：“My Student No. is ×××，and My Name is ×××”。
3. Linux 新内核的编译、安装和配置。 4. 编写应用程序以测试新的系统调用并输出测试结果。

## 四，实验步骤

### 思路

在计算中，系统调用是计算机程序从其执行的操作系统内核请求服务的编程方式。这可能包括与硬件相关的服务（例如，访问硬盘驱动器）、新进程的创建和执行以及与集成内核服务（如进程调度）的通信。系统调用提供了进程和操作系统之间的基本接口。

除了一些嵌入式系统之外，大多数现代处理器的体系结构都涉及到一个安全模型。例如，环模型规定了软件可以在多个特权级别下执行：一个程序通常被限制在它自己的地址空间，这样它就不能访问或修改其他正在运行的程序或操作系统本身，并且通常被阻止直接操作硬件设备（如帧缓冲区或网络）设备）。

通常，系统提供一个库或API，位于正常程序和操作系统之间。在类Unix系统上，该API通常是C库（libc）实现（如glibc）的一部分，glibc为系统调用提供包装函数，通常与它们调用的系统调用命名相同。在Windows NT上，该API是nt dll.dll库中本机API的一部分；这是常规Windows API的实现所使用的未经记录的API，并且直接由Windows上的某些系统程序使用。库的包装函数公开了使用系统调用的普通函数调用约定（程序集级别的子例程调用），并使系统调用更模块化。这里，包装器的主要功能是将要传递给系统调用的所有参数放在适当的处理器寄存器中（也可能放在调用堆栈上），并为内核设置一个唯一的系统调用号。这样，在操作系统和应用程序之间存在的库就增加了可移植性。

## 步骤

### 系统版本

deepin-15.9-amd64

### 原内核版本

4.15.0-29deepin-generic



![1552918656107](1552918656107.png)

### 后内核版本

4.19.29

![1552919437700](1552919437700.png)



### 具体步骤

1. [下载](https://www.kernel.org/)Linux[内核4.19.29](https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.19.29.tar.xz)

![1552919597135](1552919597135.png)

![1552919694354](1552919694354.png)

2. 解压


> xz -d linux-4.19.29.tar.xz
> tar -xf linux-4.19.29.tar



![1552919828459](1552919828459.png)

1. 将源代码移动到/usr/src文件夹下

2. 安装一些后面要用到的依赖


笔者在安装时未记录缺少的依赖，应当根据需要安装（安装时提示缺少什么就安装什么）
> sudo apt install ncurses-static.x86_64
> sudo apt install openssl-static.x86_64
> sudo apt install libelf-dev



![1552920032262](1552920032262.png)

1. 进入系统调用入口表（记录了System Call的一些基本信息）


> vim ./arch/x86/entry/systemCallAdds/systemCallAdd_64.tbl

​	添加自定义的系统调用

| 调用号 | abi |调用名称|入口（函数名）|
| ------ | ---- | ---- | ---- |
| 548 | common | hanyuu | sys_hanyuu |



![1552925585079](1552925585079.png)

1. 添加系统调用声明


> vim ./include/linux/systemCallAdds.h


若函数无参数则填写void

![1552925987245](1552925987245.png)



7. 实现系统调用

./kernel/sys.c

> 内核态下应当使用printk()
>
> 用户态下使用printf()



![1552926385840](1552926385840.png)

8. 编译内核

预处理
> sudo make menuconfig


无需裁剪内核或者更改直接两次按ESC退出即可

![1552926640804](1552926640804.png)

1. 编译内核

> j[n]指示同时允许n个任务参与编译，请根据配置调整，笔者此处配置为
>
> * CPU: Intel 8500 (6C6T all for Hyper-V)
> * Memory: 40G (15G for Deepin in Hyper-V)
> * Disk: Samsung 850 EVO (60G virtual disk)
>
> 在Hyper-V虚拟机中该配置表现较为良好




> sudo make -j64
截图为完成过后重新演示截图，此处j32，效率稍低





![1552927338919](1552927338919.png)


> 漫长的编译时光……还是挺吃配置的……

![1552927663105](1552927663105.png)


> sudo make modules_install

![1552927440962](1552927440962.png)


> make install
安装编译好的内核


a. 编译完成后，重启


> sudo reboot


b. 编写程序调用定义好的System Call

> printk()处于内核态不会直接在屏幕上打印，但是他的输出可以在/proc/kmsg目录下查看

![1552928540875](1552928540875.png)


使用dmesg也可以查看系统调用。

​![1552920832262](1552920832262.png)



## 五，主要数据结构及其说明

Linux 内核包含已知的所有系统调用的列表, 即所谓的 **系统调用表(System call)**。 每个系统调用都分配一个唯一的数字和一个内核内部函数, 负责实际完成所需的任务。若要执行系统调用, 所需呼叫的数量存储在 CPU 的EAX 寄存器中, 然后触发软件中断 128。根据 FastCall 调用约定, 系统调用上的参数将传入 CPU 寄存器中。

软件异常 在用户模式下中断程序执行, 并强制在内核模式下执行异常处理程序。这将确保上下文从无特权的环更改为环0。被称为异常处理程序是内核中的一个函数, 它读出 EAX 寄存器, 然后, 如果它包含有效的系统调用号码, 则从系统调用表中相应的内核函数与后续寄存器调用参数。在查看参数后, 从 "用户" 模式请求的任务最终由内核完成。如果此功能返回, 异常处理程序也将成功完成, 正常的程序流将继续在无特权模式下。


> mov $6, %eax ; close() ist Systemaufruf 6
> mov $15, %ebx ; Dateideskriptor als erstes Argument
> int $0x80  ; Softwareinterrupt


## 六，程序运行时的初值和结果

无初值，结果为printk()函数中的内容，截图见[上文](#具体步骤)。

## 七，实验体会

System call 作为沟通上下层的连接件，其实现没有想象中的那么难也不算简单，只要敢于尝试就会有所收获。

## 八，源程序并附上注释



* ./arch/x86/entry/systemCallAdds/systemCallAdd_64.tbl


> 548	common hanyuu sys_hanyuu //添加系统调用入口表




* include/linux/systemCallAdds.h


> amslink sys_hanyuu(void);	//自定义系统调用声明

* kernel/sys.c
```cpp
/*
* Hanyuu's system call
*/
asmlinkage long sys_hanyuu(void)	//自定义系统调用实现
{
printk("\n[[hanyuu]] System call start\n");
printk("my student number is [N/A] and my name ls Hanyuu Furude.");
printk("\n[[hanyuu]] System call end\n");
return 0;
}

hanyuu.c

// 测试程序
#include <unisted.h>
int main(){
systemcall(548);	//调用自定义系统调用
return 0;
}
```