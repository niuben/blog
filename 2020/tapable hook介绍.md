## tapable hook介绍

`tapable`是`webpack`重要的底层模块，`tapable`主要提供各式各样的hook。主要有下面几种类型：

* SyncHook
* SyncBailHook
* SyncWaterfallHook
* SyncLoopHook
* AsyncParallelHook
* AsyncParallelBailHook
* AsyncSeriesHook
* AsyncSeriesBailHook
* AsyncSeriesWaterfallHook

钩子的类型比较多，通过下面分类看的会更加直观

![tapable 钩子列表](https://user-images.githubusercontent.com/14739234/76166340-b6eb0400-6198-11ea-940e-c2b3e7482aa8.png)
[点击查看图片来源](https://github.com/zgfang1993/blog/issues/41)


下面是每个Hook的流程图和介绍

![synchook](../static/tapable/syncHook.png)

![syncbailhook](../static/tapable/syncbailhook.png)
![syncwaterfallhook](../static/tapable/syncwaterfallhook.png)
![syncloophook](../static/tapable/syncloophook.png)
![asyncparallelhook](../static/tapable/asyncparallelhook.png)
![asyncparallelbailhook](../static/tapable/asyncparallelbailhook.png)
![asyncserieshook](../static/tapable/asyncserieshook.png)
![asyncseriesbailhook](../static/tapable/asyncseriesbailhook.png)
![asyncserieswaterfallhook](../static/tapable/asyncserieswaterfallhook.png)