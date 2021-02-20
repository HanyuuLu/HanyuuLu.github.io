---
title: Tensorflow参数参阅
date: 2019-01-30 10:49:28
tags:
---
## Tensorflow变量类型
|Name|Useage|
|----|------|
|[tf.Variable](https://www.tensorflow.org/api_docs/python/tf/Variable?hl=zh-CN)|Tensor变量|
|[tf.constant](https://www.tensorflow.org/api_docs/python/tf/constant?hl=zh-CN)|Tensor常量|
|[tf.placeholder](https://www.tensorflow.org/api_docs/python/tf/placeholder?hl=zh-CN)|Tensor占位符|
|[tf.SparseTensor](https://www.tensorflow.org/api_docs/python/tf/SparseTensor?hl=zh-CN)|Tensor稀疏张量
## 设备管理
### 查看设备列表
``` py
import os
from tensorflow.python.client import device_lib
os.environ["TF_CPP_MIN_LOG_LEVEL"] = "99"
print(device_lib.list_local_devices())
```
样例输出：
``` py
[name: "/device:CPU:0"
device_type: "CPU"
memory_limit: 268435456
locality {
}
incarnation: 16586473374130916263
, name: "/device:GPU:0"
device_type: "GPU"
memory_limit: 1418693427
locality {
  bus_id: 1
  links {
  }
}
incarnation: 9802250829700710596
physical_device_desc: "device: 0, name: GeForce GTX 1050 Ti, pci bus id: 0000:01:00.0, compute capability: 6.1"
]
```
### 指定某设备进行计算
``` py
import tensorflow as tf
#选择设备 CPU->CPU:0
with tf.device('/gpu:1'):
    # '/gpu:0'
    # '/cpu:0'
    v1 = tf.constant([1.0, 2.0, 3.0], shape=[3], name='v1')
    v2 = tf.constant([1.0, 2.0, 3.0], shape=[3], name='v2')
    sumV12 = v1 + v2
    #config=tf.ConfigProto(log_device_placement=True)打印执行操作所用的设备
    with tf.Session(config=tf.ConfigProto(log_device_placement=True)) as sess:
        print sess.run(sumV12)
```
### 使用GPU计算要求
若要使用Tensorflow-gpu，请检查您是否有**算力大于3的NVIDIA显卡**。
* 查询显卡算力[地址](https://developer.nvidia.com/cuda-gpus#collapseOne)
* 若要使用CUDA加速计算，请确保您已安装[CUDA Toolkit](https://developer.nvidia.com/cuda-downloads),并且按需下载并配置您需要的[Deep learning frameworks](https://developer.nvidia.com/deep-learning-software),目前，我们用到的frameworks有[cuDNN](https://developer.nvidia.com/cudnn),请确保您的framework和CUDA版本配套，否则**无法使用**。

## 使用交互式环境
## 查看Tensor详细情况
``` py
    #使用CPU进行计算
    with tf.device("/cpu:0"):
        a = tf.constant([1.0,2.0,3.0,4.0,5.0,6.0],shape=[2,3])
        b = tf.constant([1.0,2.0,3.0,4.0,5.0,6.0],shape=[3,2])
        c = tf.matmul(a,b)
        #查看计算时硬件的使用情况
        sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
        print(sess.run(c))
#设置运行时候的参数
        options = tf.RunOptions(output_partition_graphs=True)
        metadata = tf.RunMetadata()
        c_val = sess.run(c,options=options,run_metadata=metadata)
        print(metadata.partition_graphs)
        #关闭session
        sess.close()
```
## ImportError: No module named input_data
	由于版本更新，Tensorflow已经不建议再使用input_data.如果需要继续使用，请查看[input_data.py](../Example/input_data.py)


## TensorFlow结构
- 使用图 (graph) 来表示计算任务.
- 在被称之为 会话 (Session) 的上下文 (context) 中执行图.
- 使用 tensor 表示数据.
- 通过 变量 (Variable) 维护状态.
- 使用 feed 和 fetch 可以为任意的操作(arbitrary operation) 赋值或者从其中获取数据.

## 基本操作符
``` py
import tensorflow as tf
```
* 常量
``` py
import tensorflow as tf
node0 = tf.constant(3.0,dtype=tf.float32)
node1 = tf.constant(3.0,dtype)# also tf.float32 implicitly
```
* 会话
``` py
sess = tf.Session()
print(sess.run([node0,node1]))
```
* 相加计算
``` py
node2 = tf.add(node0, node1)
print('node2', sess.run(node2))
```
* 矩阵乘法
``` py
node2 = tf.matmul(node0,node1)
```
* Placeholder占位符
``` py
a = tf.placeholder(tf.float32)
b = tf.placeholder(tf.float32)
adder_node = a + b  # + provides a shortcut for tf.add(a, b)
```
* feed_dict
```  py
print(sess.run(adder_node, {a:3, b:4.5}))
print(sess.run(adder_node, {a: [1,3], b: [2,4]}))
```
* 变量
``` py
W = tf.Variable([.3], dtype=tf.float32)
b = tf.Variable([-.3], dtype=tf.float32)
x = tf.placeholder(tf.float32)
linear_model = W*x + b
```
* 初始化变量
``` py
init = tf.global_variables_initializer()\
```
* 求值
``` py
print(sess.run(linear_model, {x: [1,2,3,4]}))
```
* 结束会话
``` py
sess.close()
```
* 降维求和
··· py
loss = tf.reduce_sum()
```
* 损失函数
``` py
y = tf.placeholder(tf.float32)
squared_deltas = tf.square(linear_model - y)    #损失函数(y(hat)-y)^2
loss = tf.reduce_sum(squared_deltas)            #减小Loss
print(sess.run(loss, {x: [1,2,3,4], y: [0, -1, -2, -3]}))
```
* 梯度下降
``` py
optimizer = tf.train.GradientDescentOptimizer(0.01) #梯度下降法
train = optimizer.minimize(loss)                    #设定损失函数目标
sess.run(init)# reset values to incorrect defaults. #初始化变量
for i in range(1000):
   sess.run(train, {x: [1,2,3,4], y: [0, -1, -2, -3]})#迭代优化
   #print(sess.run([W, b]))
print(sess.run([W, b]))                             #现实更新后的W,b的值
```
> * tf.argmax
>``` py
>tf.argmax(input, axis=None, name=None, dimension=None)
>```
>   此函数是对矩阵按行或列计算最大值
>
>  ### 参数
>   * input：输入Tensor
>   * axis：0表示按列，1表示按行
>   * name：名称
>   * dimension：和axis功能一样，默认axis取值优先。新加的字段
>返回：Tensor  一般是行或列的最大值下标向量

> * tf.cast
> ``` py
>   a = tf.Variable([1,0,0,1,1])
>   b = tf.cast(a,dtype=tf.bool)
>   sess = tf.Session()
>   sess.run(tf.initialize_all_variables())
>   print(sess.run(b))
>   [ True False False True True]
> ```
>   cast(x, dtype, name=None)\
>   将x的数据格式转化成dtype.例如，原来x的数据格式是bool，\
>   那么将其转化成float以后，就能够将其转化成0和1的序列。反之也可以