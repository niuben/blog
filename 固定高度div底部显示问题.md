---
layout: post
title: 固定高度div底部显示问题
date: 2013/01/05 19:33
tags: css 
comments: true
---

## margin-top

```html
<span style="white-space:pre">  </span>
```
```css
fixHeight {  
        height: 200px;  
        border: 1px solid #F00;  
    }  
    .fixHeight .bottomShow{  
        margin-top: 160px;  
    }  
```   
```html

<div class="fixHeight">  
<p class="bottomShow">margin-top</p>  
</div>  
```
这个方法不足时，如果修改了p的高度，需要改大量的css。

## absolute 绝对定位


```css
.fixHeight1 {  
        border: 1px solid #F00;  
        position: relative;       
        height: 200px;  
        text-align: center;  
        overflow: hidden;  
    }  
    .fixHeight1 .bottomShow{  
        position: absolute;  
        left: 0px;  
        bottom: 10px;  
        width: 100%;  
    }  
```
```html
<span style="white-space:pre">  </span>
<div class="fixHeight1">  
    <span style="white-space:pre">  </span>
    <p class="bottomShow">bottom</p>  
	<span style="white-space:pre">  </span>
</div>
```  
这个方法不需要修改子元素的高度，不足之处在于改变元素的文档结构

<!-- more -->

## padding-top


```css
.fixHeight2 {  
    height: 30px;  
        padding-top:170px;  
        border: 1px solid #F00;  
    }  
    .fixHeight2 .bottomShow{  
        height: 30px;  
        line-height: 30px;  
    }
```

```html
<span style="white-space:pre">  </span><div class="fixHeight2">  
    <span style="white-space:pre">  </span><p class="bottomShow">padding-top</p>  
<span style="white-space:pre">  </span></div> 
```

这个方法需要修改东西比margin-top还要多

## table-cell


```css
.fixHeight3 {  
        display: table-cell;  
        margin-top: 5px;  
        height: 200px;  
        width: 100%;  
        vertical-align: bottom;  
        border: 1px solid #F00;  
    }
```
```html
<span style="white-space:pre">  </span><div class="fixHeight3">  
    <span style="white-space:pre">  </span><p class="bottomShow">底部显示</p>  
<span style="white-space:pre">  </span></div>  
```
这个方法的好处是不用再设置子元素，坏处是不兼容IE6、IE7。