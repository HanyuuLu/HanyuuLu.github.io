---
title: Python进程池、线程池和进程间通信实践参考
date: 2020-08-21 00:00:00
tags:
	- Python
	- multprocessing
	- multthreading
---
## 业务需求

业务中有一个计算量比较大的、可以并行执行的操作，为了提升资源利用率，故考虑多进程分配。

性能：多线程<线程池<多进程<进程池

> 在Windows中创建一个进程耗费的时间以秒计算，故要避免频繁的进程创建销毁

### 业务流程

* 一个数据产生源，串行产生数据，速度快
* 数据源产生的数据需要比较长的时间进行计算才能得到结果
* 所有结果要汇总起来进行过滤和处理最终发放下游服务器

### 结构设计

* 每个核心分配一个处理进程、附加两个低资源占用低工作进程负责产生数据和发送数据
* 使用阻塞式的多进程间队列
  * 当任务队列满时数据产生进程阻塞暂时不产生数据（处理不及时）
  * 当结果队列满时数据处理队列阻塞不产生数据（网络问题或故障）
    * 网络请求使用多线程发送，避免过多资源占用低同时保证请求异步不阻塞
* 抽离业务代码的进程池和多线程设计代码如下

``` python
import os
import multiprocessing
import threading
import time
import json

# 发送请求地址
CONNECTION_URL = 'url:'
# CPU核心数
CPU_COUNT = os.cpu_count()
# 工作进程数
PROC_COUNT = CPU_COUNT + 2

# 发送更新数据请求
def upload(msg: dict):
    r = requests.post(CONNECTION_URL, json=msg)

# 多进程任务
def process(i, frameQueue, resQueue): 
    print(f"[proc {i}] pid {os.getpid()}")
    # fetch frame and push to work queqe
    if i == 0:
			while True:
        #.do some work
        frame =.work()
      	frameQueue.put(frame)
    # fetch result from resQueue and post it to server by multthreading
    elif i == 1:
        while True:
            res = resQueue.get()
            # do some work
            data = work2(res)
            # 多线程发送请求
            threading.Thread(target=upload, args=(data,)).start()

   # data processer process, cost many resources and require multprocessing to speed up
    else:
        while True:
            frame = frameQueue.get()
            res = work3(frame)
            resQueue.put(res)


def client():
    print(f"Process :{PROC_COUNT}")
    # 进程池创建
    pool = multiprocessing.Pool(PROC_COUNT)
    # 任务队列
    frameQueue = multiprocessing.Manager().Queue(PROC_COUNT)
    # 结果队列
    resQueue = multiprocessing.Manager().Queue(PROC_COUNT)
    # 给进程池分配函数
    for i in range(PROC_COUNT):
        pool.apply_async(
            process, (i, frameQueue, resQueue))
    # 关闭进程池不再接受新进程
    pool.close()
    # 阻塞主进程
    pool.join()
    print("finish")


if __name__ == '__main__':
    client()
```

