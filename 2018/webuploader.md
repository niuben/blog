## webuploader 
这是百度前端团队出品一个上传组件。具有限制文件大小、分片上传等功能;


### create
创建一个上传实例；

```javascript
WebUploader.create({
    // swf文件路径
    swf: '../assets/dist/Uploader.swf',
    // 文件接收服务端。
    server: uploadBaseUrl + 'open/?route=upload.feeds',
    // 选择文件的按钮。可选。
    // 内部根据当前运行是创建，可能是input元素，也可能是flash.  
    pick: {
        id: this.id,
        multiple: false
    },

    // 不压缩image, 默认如果是jpeg，文件上传前会压缩一把再上传！
    resize: false,

    fileVal: 'Filedata',

    duplicate: true,
        
    accept: {
    title: 'Images',
    extensions: 'jpg,png',
    mimeTypes: 'image/*'
    },
    compress: false            
});
```


### 配置
常用API

* formData: 用于传递参数;
* accept: 接受图片类型;


### 事件

* success:
* error:
* queen:


### 动作

* upload:


### 常见问题

#### 多一个页面请求

```html
<div id="picker">
    上传按钮
    <img src="#"/>
</div>
```
这种情况下回触发一个页面请求（先记录下来，具体原因要看看源码）
