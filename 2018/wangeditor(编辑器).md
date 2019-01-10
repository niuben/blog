## wangeditor
这是比较轻量级的编辑器。不过编辑器涉及到问题会很多，目前这个组件还存在一些问题。



### 插入图片
``` 
editor.cmd.do('insertHTML', "<p><img src='"+ url +"'/></p>");
```

### 遇到问题

#### IE上图片显示缩放键

可以通过下面样式禁用  

```
pointer-events: none;    
```
#### 复制时无法去除文本中的样式
组件暂时支持不好，需要自己做两个地方兼容
1. 绑定`paste`事件,通过获取剪切板文本内容进行追加
2. 设置`pasteTextHandle`事件返回值为空  

```javascript  
$("#edit").on( "paste", function (e) {
    textInit(e)
});  

function textInit(e) {
    e.preventDefault();
    var text;
    var clp = (e.originalEvent || e).clipboardData;
    if (clp === undefined || clp === null) {
        text = window.clipboardData.getData("text") || "";
        if (text !== "") {
            if (window.getSelection) {
                var newNode = document.createElement("span");
                newNode.innerHTML = text;
                window.getSelection().getRangeAt(0).insertNode(newNode);
            } else {
                document.selection.createRange().pasteHTML("<p>" + text + "</p>");
            }
        }
    } else {
        text = clp.getData('text/plain') || "";
        if (text !== "") {
            window.editor.cmd.do('insertHTML', "<p>" + text +"</p>");
        }
    }
}

editor.customConfig.pasteTextHandle = function (content, e) {
    return;
}
```
