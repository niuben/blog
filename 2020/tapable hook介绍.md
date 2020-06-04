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

![syncbailhook](../static/tapable/syncBailHook.png)

![syncWaterfallHook](../static/tapable/syncWaterfallHook.png)

![syncLoopHook](../static/tapable/syncLoopHook.png)
![asyncParallelHook](../static/tapable/asyncParallelHook.png)
![asyncParallelBailHook](../static/tapable/asyncParallelBailHook.png)
![asyncseriesHook](../static/tapable/asyncSeriesHook.png)
![asyncSeriesBailHook](../static/tapable/asyncSeriesBailHook.png)
![asyncSeriesWaterfallHook](../static/tapable/asyncSeriesWaterfallHook.png)