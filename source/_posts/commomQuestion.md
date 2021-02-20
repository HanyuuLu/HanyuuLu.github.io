---
title: 炼丹路上那些踩过的坑
date: 2019-02-10 00:00:00
tags: OpenCV, vsCode, tensorflow, confilct
---

 ## <font color=red>**使用VSCode运行openCV劝退事宜**</font>
 您若使用VSCode尝试Debug openCV-Python，<font color=red>我们**强烈**建议您立刻放弃这样的尝试</font>。因为目前VSCode插件pylint无法正常解析openCV-python.[详情请见VSCode团队官方issue](https://github.com/Microsoft/vscode/issues/46798)\
推荐使用[PyCharm](https://www.jetbrains.com/pycharm/)或者[Vim(高级玩家限定)](https://www.vim.org/)

 ## Why I have an error:`cv2.waitkey(0) & 0xff`
 * 高位的2个字节由Shift, Control, Num lock等状态表示，为了消除他们的影响统一用& 0xff清除这些信息。
* [原文摘录，戳我跳转](https://stackoverflow.com/questions/35372700/whats-0xff-for-in-cv2-waitkey1):\
The answers which have already been posted suggest that some of the unusual values obtained by waitKey are due to platform differences. Below, I propose that (at least on some platforms) the apparently odd behaviour of waitKey is due to keyboard modifiers. This post looks similar to Tomasz's answer because I initially wrote this as an edit, which was rejected.\
The keycodes returned by waitKey change depending on which modifiers are enabled. NumLock, CapsLock, and the Shift, Ctrl, and Alt keys all modify the keycode returned by waitKey by enabling certain bits above the two Least Significant Bytes. The smallest of these flags is Shift at 0x10000.\
 A modified version of the script Tomasz posted is given below:
``` py
#!/usr/bin/env python
import cv2
import sys
cv2.imshow(sys.argv[1], cv2.imread(sys.argv[1]))
res = cv2.waitKey(0)
print('You pressed %d (0x%x), 2LSB: %d (%s)' % (res, res, res % 2**16,repr(chr(res%256)) if res%256 < 128 else '?'))
    # Which give the following results:
    # q letter with NumLock:
    # You pressed 1048689 (0x100071), 2LSB: 113 ('q')
    # Escape key with CapsLock but not NumLock:
    # You pressed 131099 (0x2001b), 2LSB: 27 ('\x1b')
    # Space with Shift and NumLock:
    # You pressed 1114144 (0x110020), 2LSB: 32 (' ')
    # Right Arrow Key with Control, NumLock off:
    # You pressed 327507 (0x4ff53), 2LSB: 65363 ('S')
```
 I hope that helps to explain the unusual behaviour of waitKey and how to get the actual key pressed regardless of the state of NumLock and CapLock. From here it's relatively simple to do something like:
 ```
 ctrlPressed = 0 != res & (1 << 18)
```
 ...as the "control key" flag is bit 19. Shift is at bit 17, the state of CapsLock at bit 18, Alt is at bit 20, and NumLock is at bit 21.

## cuDNN 初始化失败

> Error : Failed to get convolution algorithm. This is probably because cuDNN failed to initialize, so try looking to see if a warning log message was printed above.
>
如果您使用的Tensorflow版本是1.12.0…… \
这可能是个tensorflow的bug、建议回退版本观望， \
[issue on github](https://github.com/tensorflow/tensorflow/issues/24828)

## %matplotlib inline 报错 <font color=red>invalid syntax</font>
%matplotlib inline 是jupyter notebook/console的语法，当你调用`matplotlib.pyplot`绘图函数`plot()`或生成figure时，在python console里面生成图像。
如果在其他地方运行，可直接注释。