## webGL介绍
* 着色器语言
* 坐标系
* 绘制
* 3D
* 光线


__[详细内容](https://1drv.ms/p/s!AlXIbur3cOI2fjsLL18G9BZm-8g)__

### 相关问题

#### WebGL、着色器语言、openGL和显卡交互流程？

* WebGL使用字符串方式将“着色器语言”植入着色器中;
* 着色器再调用openGL相关API;
* openGL再将代码翻译成机器语言交给显卡

> 这个游戏软件是可以通过OPENGL来支持的，那么软件就会告诉OPENGL我需要什么什么样的效果，需要构造一个什么什么样的3D模型并且渲染上色彩，然后OPENGL就把这些要求翻译成机器语言给显卡，显卡处理了输出到显示器。


### 拓展阅读
* [关于着色器工作方式](http://tieba.baidu.com/p/2523332653?traceid=)
* [GPU相关架构](http://www.360doc.com/content/13/1121/13/13669504_331012739.shtml)
