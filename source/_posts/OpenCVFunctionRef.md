---
title: OpenCV function reference
date: 2018-10-02 00:00:00
tags: OpenCV, vsCode
---
## 1.图像读取
* cv2.imread()
``` py
import numpy as mp
import cv2
img =cv2.imread('example.jpg',0)
```
> Use the function cv2.imread() to read an image. The image should be in the working directory or a full path of image should be given.[[/]]
> Second argument is a flag which specifies the way image should be read.\
>>cv2.IMREAD_COLOR : Loads a color image. Any transparency of image will be neglected. It is the default flag.\
>>cv2.IMREAD_GRAYSCALE : Loads image in grayscale mode\
>>cv2.IMREAD_UNCHANGED : Loads image as such including alpha channel
>>- *Instead of these three flags, you can simply pass integers 1, 0 or -1 respectively.*
>>- *Even if the image path is wrong, it won’t throw any error, but ```print img``` will give you ```None```*

## 2.图像显示
* cv2.inshow()
``` py
cv2.imshow('image',img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```
>Use the function cv2.imshow() to display an image in a window. The window automatically fits to the image size. \
>First argument is a window name which is a string. second argument is our image. You can create as many windows as you wish, but with different window names.

* cv2.waitKey()

>cv2.waitKey() is a keyboard binding function. Its argument is the time in milliseconds. The function waits for specified milliseconds for any keyboard event. If you press any key in that time, the program continues. If 0 is passed, it waits indefinitely for a key stroke. It can also be set to detect specific key strokes like, if key a is pressed etc which we will discuss below.
>> * Besides binding keyboard events this function also processes many other GUI events, so you MUST use it to actually display the image.

* cv2.destoryAllWindows()
>cv2.destroyAllWindows() simply destroys all the windows we created. If you want to destroy any specific window, use the function cv2.destroyWindow() where you pass the exact window name as the argument.
>> * There is a special case where you can already create a window and load image to it later. In that case, you can specify whether window is resizable or not. It is done with the function cv2.namedWindow(). By default, the flag is cv2.WINDOW_AUTOSIZE. But if you specify flag to be cv2.WINDOW_NORMAL, you can resize window. It will be helpful when image is too large in dimension and adding track bar to windows.

## 3.图像写入
* cv2.imwrite()
``` py
cv2.imwrite('messigray.png',img)
```
>Use the function cv2.imwrite() to save an image. \
>First argument is the file name, second argument is the image you want to save.

## 4.SumUp
``` py
import numpy as np
import cv2

img = cv2.imread('messi5.jpg',0)
cv2.imshow('image',img)
k = cv2.waitKey(0)
if k == 27:         # wait for ESC key to exit
    cv2.destroyAllWindows()
elif k == ord('s'): # wait for 's' key to save and exit
    cv2.imwrite('messigray.png',img)
    cv2.destroyAllWindows()
```

>* *If you are using a 64-bit machine, you will have to modify ```k = cv2.waitKey(0)``` line as follows : ```k = cv2.waitKey(0) & 0xFF```*

## 5.视频写入
> 由于暂时没有需求，本部分暂时未深入，[跳转到相关链接](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_gui/py_video_display/py_video_display.html)

## 6.绘画操作
### common arguments
>* img : The image where you want to draw the shapes
>* color : Color of the shape. for BGR, pass it as a tuple, eg: (255,0,0) for blue. For grayscale, just pass the scalar value.
>* thickness : Thickness of the line or circle etc. If -1 is passed for closed figures like circles, it will fill the shape. default thickness = 1
>* lineType : Type of line, whether 8-connected, anti-aliased line etc. By default, it is 8-connected. cv2.LINE_AA gives anti-aliased line which looks great for curves.


* cv2.line()
``` py
import numpy as np
import cv2

# Create a black image
img = np.zeros((512,512,3), np.uint8)

# Draw a diagonal blue line with thickness of 5 px
img = cv2.line(img,(0,0),(511,511),(255,0,0),5)
```
* cv2.circle()
``` py
img = cv2.circle(img,(447,63), 63, (0,0,255), -1)
```
* cv2.rectangle()
``` py
img = cv2.rectangle(img,(384,0),(510,128),(0,255,0),3)
```
* cv2.ellipse()
``` py
img = cv2.ellipse(img,(256,256),(100,50),0,0,180,255,-1)
```
>* To draw the ellipse, we need to pass several arguments. One argument is the center location (x,y). Next argument is axes lengths (major axis length, minor axis length). ```angle``` is the angle of rotation of ellipse in anti-clockwise direction. ```startAngle``` and ```endAngle``` denotes the starting and ending of ellipse arc measured in clockwise direction from major axis. i.e. giving values 0 and 360 gives the full ellipse. For more details, check the documentation of ```cv2.ellipse()```. Below example draws a half ellipse at the center of the image.

*  cv2.polylines()
``` py
pts = np.array([[10,5],[20,30],[70,20],[50,10]], np.int32)
pts = pts.reshape((-1,1,2))
img = cv2.polylines(img,[pts],True,(0,255,255))
```
>* To draw a polygon, first you need coordinates of vertices. Make those points into an array of shape ```ROWSx1x2``` where ROWS are number of vertices and it should be of type ```int32```. Here we draw a small polygon of with four vertices in yellow color.
>* If third argument is False, you will get a polylines joining all the points, not a closed shape.
>* ```cv2.polylines()``` can be used to draw multiple lines. Just create a list of all the lines you want to draw and pass it to the function. All lines will be drawn individually. It is more better and faster way to draw a group of lines than calling ```cv2.line()``` for each line.
* cv2.putText()
``` py
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(img,'OpenCV',(10,500), font, 4,(255,255,255),2,cv2.LINE_AA)
```
> To put texts in images, you need specify following things.
>* Text data that you want to write
>* Position coordinates of where you want put it (i.e. bottom-left corner where data starts).
>* Font type (Check cv2.putText() docs for supported fonts)
>* Font Scale (specifies the size of font)
>* regular things like color, thickness, lineType etc. For better look, lineType = cv2.LINE_AA is recommended.

* ```cv2.cvtColor()```
>Converts an image from one color space to another.
>* C++: void cvtColor(InputArray src, OutputArray dst, int code, int dstCn=0 )
> *  Python: cv2.cvtColor(src, code[, dst[, dstCn]]) → dst
> *  C: void cvCvtColor(const CvArr* src, CvArr* dst, int code)
> * s  Python: cv.CvtColor(src, dst, code) → None
>
>Parameters:
>* src – input image: 8-bit unsigned, 16-bit unsigned ( CV_16UC… ), or single-precision floating-point.
>* dst  – output image of the same size and depth as src.
>* code – color space conversion code (see the description below).
>* dstCn – number of channels in the destination image; if the parameter is 0, the number of the channels is derived automatically from src and code.
>
> The function converts an input image from one color space to another. In case of a transformation to-from RGB color space, the order of the channels should be specified explicitly (RGB or BGR). Note that the default color format in OpenCV is often referred to as RGB but it is actually BGR (the bytes are reversed). So the first byte in a standard (24-bit) color image will be an 8-bit Blue component, the second byte will be Green, and the third byte will be Red. The fourth, fifth, and sixth bytes would then be the second pixel (Blue, then Green, then Red), and so on.
## 3. example
``` py
import cv2 as cv2
import numpy as np
img=cv2.imread('a')
img = cv2.imread('example.jpg', 1)
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(img, 'OpenCV', (10, 500), font, 4, (255, 0, 0), 2, cv2.LINE_AA)
cv2.imshow('image', img)
# Create a black image
img = np.zeros((512, 512, 3), np.uint8)
cv2.imshow('image0', img)
# Draw a diagonal blue line with thickness of 5 px
img = cv2.line(img, (0, 0), (511, 511), (255, 0, 0), 5)
cv2.imshow('image1', img)
k = cv2.waitKey(0) & 0xffa
if k == 27:  # ESC
    cv2.destroyAllWindows()
elif k == ord('s'):
    cv2.imwrite('Save.jpg', img)
```