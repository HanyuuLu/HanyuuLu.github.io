---
title: Python 依赖冲突
date: 2019-02-10 11:06:59
tags: python, conflicts, package
---
笔者在安装scikit-image包时发现tensorflow import时直接崩溃，后发现scikit-image(后简称skimage)和tensorflow-gpu(后简称tensorflow)都依赖于numpy包，不幸的是，最新版本的scikit-image和tensorflow依赖的numpy包版本不相同并且互相不兼容(　o=^•ェ•)o　┏━┓，笔者也曾经在各搜索引擎寻找解决方案……无非是重装或者更新numpy版本，（然而并没有啥用.jpg）\
如果您也遇到了相同的问题……（先为您默哀一秒）
目前笔者收集到的的解决方案有如下几种：
* 更新您的冲突依赖到最新版本（未必有效，笔者这种情况就无解，但是这是代价最小的一种方法，如果能用这种办法解决就再好不过了，所以请有限尝试此办法）
* 寻找某个历史版本的包使两者使用相同的numpy版本（如果是其他包的其他依赖冲突则寻找使用冲突依赖的相同版本的包）（至于版本改动带来的功能差异……您只能对应的修改您的实现）
* 将您的实现分开使用不同的环境跑（我知道这听起来很不爽但是如果您一定要用冲突的版本可能也只有这个办法了……手动修改实现的巨擘除外）
* 使用其他包代替（听起来像废话但是这的确是个方法2333）
* 自闭（简单粗暴）（逃）
* 我没有发现而您觉得行之有效的任何方法，请务必[赐教笔者](mailto:HanyuuFurude@outlook.com)，感激不尽，orz
## 举例说明
笔者冲突的包是
> scipy 1.2.1
> tensorflow 1.12.0
被依赖的冲突的包是
> numpy
报错现象
RuntimeError: module compiled against API version 0xc but this version of numpy is 0xb
解决方法
tensorflow 1.12.0版本过高导致numpy兼容性问题(tensorflow-gpu 1.12.0还有其他已知bug,此处不表)逐级降低tensorflow版本到1.10.0之后发现问题消失可以正常使用
兼容的一组包
> scipy 1.2.1
> tensorflow 1.10.0
## 写在最后
配置环境一直都是比较玄学的问题(没有经验的情况下),如果您遇到了环境问题,请先保持冷静,保持冷静,冷静,然后,上网看看别人有没有类似的问题,如果没有的话,请耐心探索吧,这也是……一种经验吧……