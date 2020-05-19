##  目录
* 基础知识
    * canvas标签
    * context环境
    * 坐标
        * ratate
        * translate
        * scale
    * storke和fill   
    * save和restore
    * 性能
    * 事件
    * canvas和svg模式区别
    * 离屏

* 绘制
    * 路径
    * fill填充
    * 阴影样式
    * 渐变样式    
    * 矩形
    * 圆形
        * 弧形
    * 多边形
    * 贝塞尔    
    * 非零环绕规则    
    * canvas
    * 剪辑区

* 文本
    * 字体
    * 定位
    * 度量
    * stroke填充    
    
* 图片        
    * drawImage
    * getImageData
    * putImageData
    * createImage
    * imageData
    * 滤镜
    * 性能
    * 安全
    * 图片合成

* 视频
    * 播放视频

* 动画    
    * requestAnimateFrame
    * FTP
    * 动画类型
        * 时间运动
        * 定时
        * 视差

* 精灵
    * 对象
    * 绘制器
    * 动作
    * 制作器

* 数学和物理
    * 单位
        * 色值
        * 帧和秒
        * 米和像素
        * 角度和弧度                
    * 三角函数
    * 向量
        * 法向量
        * 点积
    * 物理运动
        * 重力
        * 抛物线
        * 钟摆
        * 时间轴扭曲
            * 函数
            * 运动
            * FPS

* 碰撞检测
    * 外界法
    * 光线投射法
    * 分离轴和最小向量

* 游戏开发
    * 游戏引擎

* 自定义控件
    * 圆角控件
    * 进度条控件
    * 滑动条控件
    * 图像控件

* 移动平台
    * viewport
    * 媒体特征查询
    
## 基础知识
Canvas是HTML5版本中最让人激动的功能。

### canvas标签
canvas标签有三个属性值，具有闭合标签, 同时提供不支持该元素的提示.通过ID、类名等方式可以获取到该元素。  


```html
<canvas id="canvas" height="800px" width=”800px“>
    The browser don't support canvas
</canvas>
```

```js
var canvas = document.getElementById("canvas");
```

### context
context是canvas提供绘制环境，分为2D和3D两种,一般使用的2D环境。在Canvas绘制之前一定要获取该环境后才能绘制。

```js
var context = canvas.getContext2d();
```

#### API
* isPointInPath: 判断点是否在路径范围之内;


### 坐标
坐标是canvas中非常重要的概念。它是二维坐标具有x轴和y轴，原点在canvas元素的左上角。  
不过和二维坐标不同是向上Y轴是变小，向下的Y轴是变大。  

![坐标图片](https://niuben.github.io/docs/static/images/canvas/coordinate.png "600*auto")


canvas提供三种方式来改变坐标轴，分别如下

* translate(x,y): 将坐标原点移动到x,y;
* scale(x_scalenum, y_scale): 对x和y轴进行缩放;
* rotate(radian): 对坐标轴进行旋转;


### storke和fill
canvas有两种绘制方式：画线和填充, 分别对应stroke和fill。

### save和restore

* save: 保存`canvas`当前坐标和各种状态;
* restore: 让上一次`save`保存的状态生效;(save和restore之间的设置状态都会被舍弃)

### 性能

### 事件
canvas标签和其他标签一样，可以绑定各种事件，比如mouse、touch事件等;

```
    //给canvas
    canvas.onmousedown =  function(e){
        var x = e.x || e.clientX,
        y = e.y || e.clientY,
        bbox = canvas.getBoundingClientRect();

        return {
            x: x - bbox.left * (canvas.width / bbox.width),
            y: y - bbox.top * (canvas.height / bbox.height)
        };
    }
```
### canvas和svg区别
canvas是
svg是

### 离屏
将canvas内容输出到图片中;

---
## 样式
canvas提供了多种设置样式的方法, 所有样式都设置在context对象中并具有全局性。具体方法如下：

* 字体样式
* stroke样式
* fill样式
* 阴影样式
* 渐变样式
* 图片样式


### font(字体样式)
字体样式很像css中font属性，是一组属性组合而成. 具体如下

* fontWeight: 加粗
* fontSize:  字号
* fontFamily: 字体

```
    context.font = "bold 18px sans-serif"
```

### stroke样式
提供一些属性设置线的样式，包括宽度、颜色等。具体属性如下：

* strokeWidth: 设置线的宽度;
* strokeStyle: 设置线的颜色;

```
    context.strokeWidth = "1";
    context.strokeColor = "rgba(0, 0, 0, 0.8)";
```

### fill样式
提供一些属性设置填充的样式, 包括颜色等，具体属性如下：

* fillStyle: 设置填充的颜色;

```    
    context.fillColor = "rgba(0, 0, 0, 0.8)";
```

### 阴影样式

* shadowOffsetX: 阴影偏移的X轴;
* shadowOffsetY: 阴影偏移的Y轴;
* shadowBlur: 阴影大小;


```
    context.shadowOffsetX = "2px";
    context.shadowOffsetY = "3px";
    context.shadowBlur = "5px"
```


### 渐变色样式
渐变色分为两种：线性和放射;

* createLinearGradient(startColor, endColor): 创建线性渐变
* createRadiaGradient(x1, y1, radius1, x2, y2, radius2): 创建发射性渐变


### 图片样式

* url("")

---
## 绘制

### 路径
* beginPath(): 开始一个子路径。同时清空之前的路径;
* closePath(): 结束一个路径；
* moveTo(x, y): 移动一个具体点，并同时开启一个子路径;
* lineTo(x, y); 连接到一个具体点,如果没有子路径的话，则开启一个子路径；

moveTo和lineTo区别：
1. moveTo创建一个子路径，但是lineTo不一定创建子路径。
2. moveTo创建的子路径只包含一个点。当没有子路径时，lineTo会创建一个子路径，如果有子路径的话lineTo会将点加入到子路径中。

### 填充
* fill(): 对区域进行填充;
* stroke(): 对区域进行描边;

### 矩形
* clearRect(x, y, width, height): 清空一个矩形区域;
* strokeRect(x, y, width, height): 对一个矩形区域进行描边;
* fillRect(x, y, width, height): 对一个矩形区域进行填充;

### 圆
* arc:(double x, double y, double radius, double startRadian, double endRadian, boolean counter-clockwise): 画圆;

```

```
### 多边形


### 弧形
* arcTo:(double x1, double y1, double x2, double y2, double radius)：画圆弧; 


### 贝塞尔曲线

#### 二次方贝塞尔曲线

#### 三次方贝塞尔曲线

### canvas
还可以将另一个canvas的内容绘制到自身中;

### 剪辑区
clip()方法设置剪辑区，后面的操作都在剪辑区中操作且不不影响其他区域;

---
## 文本

### 字体

### 定位
定位分为x、y轴，类似于`background-position`

* top
* center
* bottom

---
## 图像

* drawImage
* getImageData、putImageData
* createImageData
* 图像合成

### drawImage(img, sx, sy, swidth, sheight, x, y, width, height);
drawImage有多个参数，有些参数是可选的。每个参数的意思如下：

* img: 图片、视频、Canvas、DOM元素;
* sx:开始裁剪图片位置的X轴; （可选 ）
* sy: 开始裁剪图片位置的Y轴;（可选 ）
* swidth: 裁剪图片的宽度;（可选 ）
* sheight: 裁剪图片的高度;（可选 ）
* x: 设置图片在Canvas位置的X轴;
* y: 设置图片在Canvas位置的Y轴;
* width: 设置图片显示宽度；（可选 ）
* height: 设置图片显示高度；（可选 ）

通常使用drawImage有三种情况
* drawImage(img, x, y): 设置图片在Canvas上的位置，图片会自适应的显示;
* drawImage(img, x, y, width, height): 设置图片在Canvas上的位置和图片的尺寸;
* drawImage(img, sx, sy, swidth, sheight, x, y, width, height): 设置裁剪的位置和大小，设置图片显示位置和大小。如果swidth和width，sheight和height不相等，图片会以width、height的尺寸显示，从而会产生缩放和拉伸的效果。 


### getImageData()和putImageData()
使用getImageData方法可以获取图片像素，API参数如下：
* getImageData(x, y, width, height): 通过x,y指定canvas上的位置，通过width，height获取固定尺寸;

使用putImageData方法可以将图片对象，放入指定的位置并可以设置显示尺寸;
* putImageData(imgData, dx, dy, dirtyX, dirtyY, dirtyWidth, dirtyHeight): dx、dy是canvas上的坐标，dirtyX、dirtyH是图片的偏移量，dirtyWidth,dirtyHeight显示图片的尺寸大小; 

### createImageData()
创建一个imageData对象。  

```
    //imageData对象结构
    array[0] = "RGB(0, 0, 0, 100);
    array[1] = "RGB(0, 0, 0, 100);
    array[2] = "RGB(0, 0, 0, 100);
```

### 滤镜

#### 浮雕

#### 黑白

#### 

### 图像合成

将图像

#### 图像合成方式

* 
* 

### 安全
`canvas`为了安全，规定不能操作不同域下的图片;

### 度量
* measue: 计算字体的宽度;

---
## 动画

### 循环

* setTimeout和setInterval: 
* requsetAnimationFrame: 专门为`canvas`设计的API,充分利用浏览器运行周期;


通过`cancelAnimationFrame`取消循环


### 帧速率（FPS)
fps(frames per second) 一秒钟显示多少帧。 


### 基于时间运动
每台电脑的帧速率不一样的，性能好电脑FPS比较高。对于一些移动的功能，需要抹平帧速率不同带来的影响。

### 背景绘制
有些时候把背景全部删除再重新绘制的成本很高。目前有两种成本较低的方案，他们如下

* 剪辑区绘制：

* 背景覆盖:

### 双缓冲动画

### 视差动画
人的眼睛看物体规律：远的物体小移动速度慢，近的物体大移动速度快。通过这个原则可以制作出符合人眼规律的动画。

### 定时动画


---
## 精灵
将一个动画模块封装，类似一个动画的类。通过封装一些常用属性和方法来提高复用效率。

```
    function Sprite(){

        this.height = 0;
        this.width = 0;
    }

```

### 绘制器
将绘制通过参数方式传入，大大提高类的灵活性。

```
    function Sprite(){
        this.height = height;
        this.width = width;
        this.praint = paint;
    } 

    function paint(){

    }

```

### 

### 动作

---
## 碰撞检测

### 外接图形判别法
* 矩形判别法
* 圆形判别法

### 边缘判别法

### 光线投射法

### 投影法

### 分离轴方法
通过判断多边形在分离轴上投影来判断是否重合。具体步骤如下

1. 算出多边形每条边的向量（按照统一的方向）
2. 算出每个向量的投影轴;
3. 遍历投影轴，计算每个多边形的各个点在投影轴上的点积；
4. 去每个多边形点积最大值和最小值，然后是否重合。如果没有重合证明没有碰撞.

---
## 数学和物理

### 单位
        
#### 色值
* rgba()
* HSL

#### 帧数和时间

间隔（毫秒) = 1000 / fps;

fps = 1000 / 间隔（毫秒） 

#### 米和像素
设定Canvas宽度是多少米，比如10米，再通过Canvas宽度像素值除以10米，从而得到每一米有多少像素值;


#### 角度和弧度

弧度 = Math.PI / 180 * 角度

角度 = Math.PI / 弧度 * 180

### 三角函数

Sin = 对边 / 斜边

Cos = 邻边 / 斜边

### 向量
具体方向和大小;

#### 法向量
和当前向量垂直的单位向量;

#### 点积
用于判断两个向量共同作用力后的方向;

### 物理运动
        
#### 重力

#### 抛物线

#### 钟摆

### 时间轴扭曲

* linear
* ease-in
* ease-out
* ease-in-out


### 拓展阅读
- [canvas 旋转后截图和缩放](https://github.com/niuben/blog/blob/master/canvas%20%E6%97%8B%E8%BD%AC%E5%90%8E%E6%88%AA%E5%9B%BE%E5%92%8C%E7%BC%A9%E6%94%BE.md)
- [canvas 旋转图片（javascript）](https://github.com/niuben/blog/blob/master/canvas%E6%97%8B%E8%BD%AC%E5%9B%BE%E7%89%87%EF%BC%88javascript%EF%BC%89.md)