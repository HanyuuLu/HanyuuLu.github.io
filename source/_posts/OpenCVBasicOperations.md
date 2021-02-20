---
title: OpenCV basic operations
date: 2018-10-02 00:00:00
tags: OpenCV
---
## 1.Accessing and Modifying pixel values
***
``` py
import cv2
import numpy as np
# Load a color image.
img = cv2.imread('example.jpg')
px = img[100,100]
print(px)
# accessing only blue pixel
# BGR color system!
blue = img[100,100,0]
print(blue)
```
* You can access a pixel value by its row and column coordinates. For BGR image, it returns an array of Blue, Green, Red values. For grayscale image, just corresponding intensity is returned.
* You can modify the pixel values the same way.
``` py
img[100,100]=[255,255,255]
print(img[100,100])
```
* Numpy is a optimized library for fast array calculations. So simply accessing each and every pixel values and modifying it will be very slow and it is discouraged.
* Above mentioned method is normally used for selecting a region of array, say first 5 rows and last 3 columns like that. For individual pixel access, Numpy array methods, ```array.item()``` and ```array.itemset()``` is considered to be better. But it always returns a scalar. So if you want to access all **B,G,R values**, you need to call ```array.item()``` separately for all.
* example:
 ```py
# accessing RED value
img.item(10,10,2)

# modifying RED value
img.itemset((10,10,2),100)
img.item(10,10,2)
```

## 2. Accessing Image Properties
* img.shape
``` py
print(img.shape)
```
* Image properties include number of rows, columns and channels, type of image data, number of pixels etc.\
* Shape of image is accessed by img.shape. It returns a tuple of number of rows, columns and channels (if image is color).\
* If image is grayscale, tuple returned contains only number of rows and columns. So it is a good method to check if loaded image is grayscale or color image.
* img.size
``` py
print(img.size)
```
* Total number of pixels is accessed by ```img.size```.
* img.dtype
``` py
print(img.dtype)
```
* ```img.dtype``` is very important while debugging because a large number of errors in OpenCV-Python code is caused by invalid datatype.
## 3. Image ROI
* Sometimes, you will have to play with certain region of images. For eye detection in images, first perform face detection over the image until the face is found, then search within the face region for eyes. This approach improves accuracy (because eyes are always on faces :D ) and performance (because we search for a small area).
* ROI is again obtained using Numpy indexing.
``` py
clip = img[280:340, 330:390]
img[273:333, 100:160] = clip
```

## 4.Splitting and Merging Image Channels

The B,G,R channels of an image can be split into their individual planes when needed. Then, the individual channels can be merged back together to form a BGR image again. This can be performed by:
``` py
b,g,r = cv2.split(img)
img = cv2.merge((b,g,r))
```
 Or
``` py
b = img[:,:,0]
```
 Suppose, you want to make all the red pixels to zero, you need not split like this and put it equal to zero. You can simply use Numpy indexing which is faster.
``` py
# BRG color system
img[:,:,2] = 0
```
 *```cv2.split()``` is a costly operation (in terms of time), so only use it if necessary. Numpy indexing is much more efficient and should be used if possible.*
## 5.Making Borders for Images (Padding)

If you want to create a border around the image, something like a photo frame, you can use ```cv2.copyMakeBorder()``` function. But it has more applications for convolution operation, zero padding etc. This function takes following arguments:
* **src** - input image
* **top, bottom, left, right** - border width in number of pixels in corresponding directions
* **borderType - Flag defining what kind of border to be added. It can be following types:**
   * **cv2.BORDER_CONSTANT** - Adds a constant colored border. The value should be given as next argument.
   * **cv2.BORDER_REFLECT** - Border will be mirror reflection of the border elements, like this : fedcba|abcdefgh|hgfedcb
   * **cv2.BORDER_REFLECT_101** or **cv2.BORDER_DEFAULT** - Same as above, but with a slight change, like this : gfedcb|abcdefgh|gfedcba
   * **cv2.BORDER_REPLICATE** - Last element is replicated throughout, like this: aaaaaa|abcdefgh|hhhhhhh
   * **cv2.BORDER_WRAP** - Can’t explain, it will look like this : cdefgh|abcdefgh|abcdefg
* value - Color of border if border type is cv2.BORDER_CONSTANT

Below is a sample code demonstrating all these border types for better understanding:
``` py
import cv2
import numpy as np
from matplotlib import pyplot as plt

BLUE = [255,0,0]

img1 = cv2.imread('opencv_logo.png')

replicate = cv2.copyMakeBorder(img1,10,10,10,10,cv2.BORDER_REPLICATE)
reflect = cv2.copyMakeBorder(img1,10,10,10,10,cv2.BORDER_REFLECT)
reflect101 = cv2.copyMakeBorder(img1,10,10,10,10,cv2.BORDER_REFLECT_101)
wrap = cv2.copyMakeBorder(img1,10,10,10,10,cv2.BORDER_WRAP)
constant= cv2.copyMakeBorder(img1,10,10,10,10,cv2.BORDER_CONSTANT,value=BLUE)

plt.subplot(231),plt.imshow(img1,'gray'),plt.title('ORIGINAL')
plt.subplot(232),plt.imshow(replicate,'gray'),plt.title('REPLICATE')
plt.subplot(233),plt.imshow(reflect,'gray'),plt.title('REFLECT')
plt.subplot(234),plt.imshow(reflect101,'gray'),plt.title('REFLECT_101')
plt.subplot(235),plt.imshow(wrap,'gray'),plt.title('WRAP')
plt.subplot(236),plt.imshow(constant,'gray'),plt.title('CONSTANT')
```
See the result below. (Image is displayed with matplotlib. So RED and BLUE planes will be interchanged):![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/border.jpg)
{% asset_img https://opencv-python-tutroals.readthedocs.io/en/latest/_images/border.jpg %}
***
## Example
``` python
import cv2

img = cv2.imread('example.jpg')
print('img[300,300]=%s' % img[300,300])
print('blue pixel %s' %img[300,300,0])
print('green pixel %s' %img[300,300,1])
print('red pixel %s' %img[300,300,2])
print(img[300,300,3])

cv2.imshow('img', img)
k = cv2.waitKey(0)  # & 0xff
if k == 27:  # ESC
	cv2.destroyAllWindows()
elif k == ord('s'):
	cv2.imwrite('Save.jpg', img)
```