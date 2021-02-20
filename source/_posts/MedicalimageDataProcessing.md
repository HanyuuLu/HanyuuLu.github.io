---
title: Medical image Data Processing
date: 2019-02-03 00:00:00
tags: deep learning, medical, image, gile processing08-6
---
> 转载自[医疗影像数据处理--python处理nifti数据](https://zhuanlan.zhihu.com/p/23738974)
* dicom
dicom是由磁共振设备扫描产生的,一般是一个slice一个文件,
分析格式: .img/.hdr
* nifti
由fsl \ afni \ spm 共同确定的格式,支持3D,4D影响
分析格式: img/hdr
dicom -----> nifti 影像处理流程
1. 利用spm将dicom原始数据转换为 分析格式:*.img 和*.hdr
2. 利用dcm2nifti工具将dicom数据转为nifti 3d数据
3. 利用spm进行3d转为4d
处理nifti数据的可用工具：matlab\ITK\VTK\python.
我们使用python处理该类型的数据
开发工具
1. 语言python2.7
2. python module: nipy scipy nibabel matplotlib
### 获取数据形态信息及头信息
``` py
#-*- coding=utf8 -*-
import nibabel as nib
#import os
#from nibabel.testing import data_path
#d_path= os.path.join(data_path,"1.nii")
#data1= nib.load(d_path)
data1= nib.load("1.nii")
#数据形状
print data1.shape
print data1.affine.shape
img = data1.get_data()
#数据形状,矩阵数据每一维的数据尺寸
print img.shape
#数据头信息
print data1.header
```
### 获取slice信息生成图像
``` py
#-*- coding=utf8 -*-
import nibabel as nib
import matplotlib.pyplot as plt
#把slice数据生成图片的方法
def show_img(slices):
fig, axes = plt.subplots(1, len(slices))
for i, slice in enumerate(slices):
axes[i].imshow(slice.T, cmap="gray", origin="lower")
#读取nifti文件中的slice数据
data1= nib.load("/home/lee/nifti/1.nii.gz")
img = data1.get_data()
#获取单张slice数据
slice_0 = img[26, :, :]
slice_1 = img[:, 30, :]
slice_2 = img[:, :, 16]
#生成图表
show_img([slice_0, slice_1, slice_2])
plt.suptitle("show slice image")
```