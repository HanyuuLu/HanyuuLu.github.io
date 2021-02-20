---
title: Tensorflow入坑试水
date: 2019-01-30 10:49:28
tags:
---
# Leadin
## The computation graph	计算图
### Building the graph	建立一张图
``` py
import tensorflow as tf
matrix1 = tf.constant([[3.,3.]]) # create a constant
matrix2 = tf.constant([[2.],[2.]])
product = tf.matmul(matrix1,matrix2)  # create a matlum op(operation)
```
### Launching the graph in a session 在图中开启一个会话

```py
sess = tf.Session()  # create a session in tensorflow
result = sess.run(product)
print(result)
sess.close()   # DONT FORGET THIS!
```
or
``` py
with tf.Session as sess:
	# Do something
```
### Tensors 张量
### Variables 变量
### Constant 常量

``` py
import tensorflow as tf
state = tf.Variable(0, name = 'Counter')
ont = tf.constant(1)
new_value = tf.add(state, one)
update = tf.assign(state, new_value)
init_op = tf.global_variables_initializer()
# init_op = tf.initialize_all_variables()
# not recommand for that will be disabled shrotly.
with tf.Session() as sess:
	sess.run(init_op)
	print(sess.run(state))
	for _ in range(3):
		sess.run(update)
		print(sess.run([state,one,new_value]))
```
### Fetch 取回
### Feed 供给
* Placeholder 占位符
``` py
input1 = tf.placeholder(tf.float32)
input2 = tf.placeholder(tf.float32)
output = tf.add(input1,input2)
with tf.Session() as sess:
	print(sess.run([output],feed_dict = {input1:[7.], input2:[2.]}))
```