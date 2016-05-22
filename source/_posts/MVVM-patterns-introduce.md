title: MVVM模式快速入门
date: 2015-11-13 15:40:25
toc: false
tags: MVVM
categories: iOS开发
---
**MVVM开发模式是一个比较新的技术，国内对这个模式的文档大都是翻译国外的，而且文档也比较少，对于新手来说入门也是比较困难的，那么今天我就把我所理解的一些关于MVVM的思考分享一下，帮助大家快速入门。**

<!--more-->

MVC模式想必大家都很熟悉了，到处都是用这个模式，我从编程入门就一直接触这个模式，压根就不知道MVVM模式。MVC模式分为Model，View，Controller三层，Model负责数据层，Controller负责业务逻辑层，View负责界面显示层，所以可以让不同的View使用用一个Controller层，程序扩展性比较好。所以我就不必多说这个模式了好处了。

那么我就就来谈谈MVC模式的坏处：不知道大家有没有感觉自己做iOS开发的时候一个tableViewController里面的代码好多好多，自己想找个方法就要找很久，以为找到了更新UI的方法，不料却是一个网络请求的方法，my god
***


 开始吐槽：
 ===
* 你既然叫ViewController为什么网络请求的任务也要交给你
* 你既然叫ViewController为什么数据解析的任务也要交给你？
* 你既然叫ViewController为什么数据存储的任务也要交给你？
* 你既然叫ViewController为什么你管那么多事？为什么不好好负责你的View啊？？？？

### 吐槽完毕：

大家是不是看到了不好的地方，那么我们就要改MVC，怎么改呢？

* 你既然叫ViewController是吧，那么你就负责View的显示和更新，其他业务逻辑不需要你管，把你和View层绑在一起了，你们两个就负责一层就行了。

* 对于Model层，你还给我负责数据层就行了

* 那么业务逻辑层呢？

* 业务逻辑层我就在ViewController层和Model层之间再添加一个ViewModel层就行了，让他负责业务逻辑，负责网络请求和数据解析。

大家可以感受一下下面这个图：
![这里写图片描述](http://img.blog.csdn.net/20151025183218486)

这个图很好的诠释了MVC模式和MVVM模式的差别。

** 也就是说MVVM模式是MVC模式的升级版。 **

那么现在我们可以说ViewController从ViewModel层中读取数据然后显示在View上，他并不和Model层直接打交道，和Model层直接打交道的是ViewModel层。

下面我们就解释一下MVVM模式每层之间怎么通讯的

![这里写图片描述](http://img.blog.csdn.net/20151025184019212)

其实ViewController中会包含一个viewmodel的对象，View层需要变化，可以直接让这个对象调用ViewModel的方法获取数据，ViewModel层获得数据然后保存Model中，但是ViewModel层获取的数据怎么才能告诉ViewController层刷新UI呢？

![这里写图片描述](http://img.blog.csdn.net/20151025191727603)

** 方法1：使用block回调 **

可以在获取数据调用一个block回调，然后在ViewController中更新UI数据
在swift中是闭包。

```
//获取数据然后有一个block回调
-(void)topRefreshWithCallBack: (callback)callback;
```

** 方法2：使用reactiveCocoa库 **

github地址：[reactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa)
这个库包含了函数式编程和响应式编程。其实reactiveCocoa库就是MVVM模式的核心。
用法可以参考：
[ReactiveCocoa入门教程——第一部分](http://benbeng.leanote.com/post/ReactiveCocoaTutorial-part1)
[ReactiveCocoa入门教程——第二部分](http://benbeng.leanote.com/post/ReactiveCocoaTutorial-part2)

关于MVVM更好的介绍，请参考我实现的一个小demo
[请使出九牛二虎之力来点击我](https://github.com/CoderTian/DemoWithMVVM)

其他资料的参考
http://www.sprynthesis.com/2014/12/06/reactivecocoa-mvvm-introduction/
http://blog.scottlogic.com/2014/07/24/mvvm-reactivecocoa-swift.html

