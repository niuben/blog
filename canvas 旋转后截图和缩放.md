---
layout: post
title: canvas 旋转后截图和缩放
date: 2013/01/05 19:33
tags: canvas 
comments: true
---

上次说了，当一个图片旋转的时候，画布必须为一个正方形，这样旋转的时候，图片就不会被截取掉。但是这样的话就造成了另一个问题，就是画布中有空白的区域。这篇文章，我们就来解决空白区域的问题。

首先介绍一个函数：getImageData(x,y,w,h)得到画布特定区域的图片信息。参数 分别指的是画布起始点的x轴和y轴，区域的高度和宽度。

 另一个函数：putImageData(imageData,x,y,dx,dx,dw,dh) 将得到的图像数据放入画布中。

 当图片（长方形）旋转的时候的，图片在画布中的位置是变化的。

正常： 90度：180度： 270度：

刚才是宽度大于高度的情况，下面是宽度小于高度的情况。

正常： 90度： 180度： 270度：

看上面的把张图片，我们可以看出图片不一样的，需要截取的位置就不一样。一般来说就有两种情况：宽度大于高度，宽度小于高度。我们要根据特定的情况，来获得位置。

得到数据以后，后面的工作就简单了。

不过还有一种情况，是必须要考虑的，就是图片的尺寸大于屏幕的尺寸。这种情况下，我们就必须把图片进行一些缩放。缩放时候，我们用到了drawImage这个函数。我们首先来看看这个函数。drawImage(image,sx,sh,sw,sh,dx,dy,dh,dw)。

当dh、dw大于sh、sw，那么图片就会被缩小，反之就会被放大。
<!-- more -->
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">  
<html xmlns="http://www.w3.org/1999/xhtml">  
<head>  
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />  
<title>图片旋转</title>  
<style type="text/css">  
    canvas,img{  
        border:1px solid #F00;  
    }  
</style>  
</head>  
  
<body>  
<p><input type="button" id="rotate" value="旋转" onclick="ratateImage()"/><input type="button" id="tranforms" value="转换"/></p>  
<canvas id="myCanvas" width="117" height="117"></canvas>  
<img src="#" id="show" />  
<script type="text/javascript" >  
var rotate=0;  
function ratateImage(){       
    var c=document.getElementById("myCanvas");  
    var img=new Image();  
    img.src="test2.PNG";  
    img.onload=function(){  
        var zoomNum=1;  
        if(this.width>117){  
            var zoomNum=117/this.width;  
        }  
        var sizeNum=this.height>this.width?this.height:this.width;  
        sizeNum=sizeNum*zoomNum;      
        var centerNum=Math.ceil(sizeNum/2);      
        c.setAttribute("width",sizeNum);  
        c.setAttribute("height",sizeNum);     
        var cxt=c.getContext("2d");  
        cxt.translate(centerNum,centerNum);       
        cxt.rotate(getRotateNum());           
        cxt.drawImage(img,0,0,this.width,this.height,-centerNum,-centerNum,this.width*zoomNum,this.height*zoomNum);  
        getPosition(this);  
    }  
    rotate+=90;  
    if(rotate==360){  
        rotate=0;  
    }  
}  
function getRotateNum(){          
    var rotate=this.rotate;  
    var reg=Math.PI/2;  
    switch(rotate){  
        case 90:reg=reg;  
        break;  
        case 180:reg=reg*2;  
        break;  
        case 270:reg=reg*3;  
        break;              
    }          
    return reg;                  
}  
  
function getPosition(imgObj){  
    var zoomNum=117/imgObj.width;      
    var sizeNum=imgObj.height>imgObj.width?imgObj.height:imgObj.width;          
    if(imgObj.width<imgObj.height){  
        if(rotate==90){  
            _x=0;  
            _y=0;  
            _w=imgObj.height*zoomNum;  
            _h=imgObj.width*zoomNum;          
        }else if(rotate==180){  
            _x=sizeNum-imgObj.width*zoomNum;  
            _y=0;  
            _w=imgObj.width*zoomNum;  
            _h=imgObj.height*zoomNum;  
        }else if(rotate==270){  
            _x=0;  
            _y=sizeNum-imgObj.width*zoomNum;  
            _w=imgObj.height*zoomNum;  
            _h=imgObj.width*zoomNum;  
        }else{  
            _x=0;  
            _y=0;  
            _w=imgObj.width*zoomNum;  
            _h=imgObj.height*zoomNum;  
        }         
    }else{        
        if(rotate==90){  
            _x=sizeNum-imgObj.height*zoomNum;  
            _y=0;  
            _w=imgObj.height*zoomNum;  
            _h=imgObj.width*zoomNum;          
        }else if(rotate==180){  
            _x=0;  
            _y=sizeNum-imgObj.height*zoomNum;  
            _w=imgObj.width*zoomNum;  
            _h=imgObj.height*zoomNum;  
        }else if(rotate==270){  
            _x=0;  
            _y=0;  
            _w=imgObj.height*zoomNum;  
            _h=imgObj.width*zoomNum;  
        }else{  
            _x=0;  
            _y=0;  
            _w=imgObj.width*zoomNum;  
            _h=imgObj.height*zoomNum;  
        }     
    }  
    posObj={  
            x:Math.floor(_x),  
            y:Math.floor(_y),  
            w:Math.floor(_w),  
            h:Math.floor(_h)  
    };  
    return posObj;  
}  
  
ratateImage();  
  
document.getElementById("tranforms").onclick=function(){  
    debugger;  
    var e=document.getElementById("myCanvas");  
    cxt=e.getContext("2d");   
    alert(posObj.x+","+posObj.y+","+posObj.w+","+posObj.h);  
    var imageData=cxt.getImageData(posObj.x,posObj.y,posObj.w,posObj.h);  
    e.setAttribute("height",_h);  
    e.setAttribute("width",_w);   
    cxt.clearRect(_h,_w);              
    cxt.putImageData(imageData,0,0);  
    var b=e.toDataURL();  
    document.getElementById("show").src=b;  
}  
  
</script>  
</body>  
<script></script>  
</html>  
```