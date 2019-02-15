## openCV.js

### 数据类型
* Mat
    * At 
        * UcharAt
        * CharAt
        * UshortAt
        * ShortAt
        * IntAt
        * FloatAt
        * DoubleAt
    * Ptr
        * UcharPtr
        * CharPtr
        * UshortPtr
        * ShortPtr
        * IntPtr
        * FloatPtr
        * DoublePtr
* Array
    * Unit8Array
    * Int8Array
    * Unit16Array
    * Int16Array
    * Int32Array
    * Float32Array
    * Float64Array
* Object
* Boole
* Int
    * Int16
    * Int32
* Float
    * Float16
    * Float32    

### 颜色
* RGB:
* Gray:
* HSV:

### 常量
* COLOR_RGBA2GRAY
* CV_8UC4
* ColorConversionCodes
* INTER_LINEAR
* ROTATE_90_CLOCKWISE 
* ROTATE_180 
* ROTATE_90_COUNTERCLOCKWISE
* CHAIN_APPROX_SIMPLE
* TM_SQDIFF

### 形状
* Point: 点
* Scalar:
* Size:
* Rect: 
* Circle: 
* RotatedRect: 

在JavaScript中，Scalar是用数组表示，Point、Scalar、Size、Rect、Circle、RotateRect是用对象表示的;

### API

* inRange(): 在RGB中取一定范围内的值;
* resize():


### 功能
* GUI功能
    * subtract(): 将两个RGB值相减;
    * add(): 将两个RGB值相加;
* 边缘检测: 
    * canny(): 边缘检测;
    * cvtColor(): 
* 图像过滤: 
    * warpAffine():
    * getAffineTransform():
    * warpPerspective():
    * bilateralFilter(): 双边过滤;
* 图像金字塔(Image Pyramid):

* 图像轮廓:
    * findContours():  
    * drawContours(): 
* 图像直方图:

* 图像转换: 傅里叶转换、余弦转换
    * 
* 模板匹配:
    * templateMatch():
    * minMaxLoc():
* 图像分割(Image Segmentation):
    * watershed  分水岭算法
* 图像前置主体提取
    * Grabcut:

### 功能

#### 图片美化



### 其他文章

* [openCV入门介绍](https://github.com/niuben/blog/blob/master/2018/openCV入门介绍.md)