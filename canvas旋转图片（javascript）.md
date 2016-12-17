---
layout: post
title: canvas旋转图片（javascript）
date: 2013/03/06 10:29
tags: canvas 
comments: true
---
 
最近在项目中需要用canvas旋转图片，这方面一开始也比较不懂。找了很多的资料才搞定这个问题。主要用到了三个函数，translate、rotate、drawImage。

* translate(x,y)：移动画布坐标系统，x和y表示水平和竖直方向的偏移量。
* rotate(reg)：旋转图片，reg表示旋转的度数。比如：Math.PI/4 表示为45。
* drawImage(source,sx,sy,sh,sw,dx,dy,dh,dw)：将图片加载到画布上。source表示的图片的对象，sx和sy表示距图片圆点的偏移量，sh和sw表示加载图片的长度和宽度。dx和dy：距画布原点的偏移量，dh和dw：利用画布宽度和高度。

现在我们要将旋转一张图片，顺时针进行旋转。

首先要在HTML上面增加一个canvas元素 

```html 
<canvas id="test" width="120" height="120"></canvas>
```

然后需要在JavaScript中获取这个canvas

```js
var canvasObj = document.getElementById("test");
```
下面我们则要返回canvas绘图环境

```js
var cxt = canvasObj.getContext("2d"); //2d表示用二维的方式
```
这时候我们要给这个画布设定一个旋转中心点。不过这个地方要重点说一下，图片一般都是长方形的。如果画布也设置成长方形的话。旋转过程中，有一部分图片会被截取掉。所以当图片为长方形的时候，我们先要将以图片最大尺寸的边来设置画布的长度和宽度。比如一张图片是200*100，那我们的画布必须是200*200。这样旋转的时候，才不会被截断。

设置中心点，就是用上面说的translate方法，x和y设置成正方形的中心点。比如200*200的画布，中心点则是100*100

```js
 cxt.translate(100,100);
```
 设置好中心点以后，我需要将画布进行旋转。 

```js
 cxt.rotate((2*Math.PI)/*4); //旋转90度
```
 然后我们增加一个图片对象。

```js
var img = new Image(); 
img.src = "baidu_jgylogo3.gif";
img.onload = function(){
	cxt.drawImage(this,-100,-100); //-100和translate的正向100，相互抵消。从而使图片的原点和画布的原点重合。         
}
```
这样就完成图片旋转的功能。

代码：

```js
var c = document.getElementById("myCanvas");  
var cxt = c.getContext("2d");  
cxt.translate(58,58);  
cxt.rotate( 2 * (Math.PI / 2));  
var img = new Image();  
img.src = "baidu_jgylogo3.gif";  
img.onload = function(){    
    cxt.drawImage(img,-58,-58);   
    cxt.save();
}  

```

