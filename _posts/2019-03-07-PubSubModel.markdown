---
layout:       post
title:        "常用设计模式"
subtitle:     "发布订阅模式"
date:         2019-03-09 21:00:00
author:       "boowen"
header-img:   "img/post-bg-module.png"
header-mask:  0.3
tags:
    - 前端
    - JS
    - 设计模式
---
>其实设计模式并没有那么高深，它只是一种写代码的套路。或许在写代码的时候，你已经用了某种设计模式，但是并不知道它是有名字的。现在总结下常用的设计模式。以后方便装逼。

## 学习目的

>学习一样东西的目的，自然是要解决实际的问题。

我在写项目的时候常常遇见的一个场景，在请求数据后，需要刷新场景的模型、播放的状态、预览设置、表格的数据、标题栏等多个地方的数据（当时没有用VUE）。刚开始的拿到需求，很快就写好了，代码如下：

```
login.succ(function(data){
    //请求数据成功后
    sence.handleData(data); // 通知设置场景
    playModule.handleData(data); // 通知播放模块
    persetModule.handleData(data); // 通知预览设置
    tableModule.handleData(data); // 通知表格模块
});
```
刚开始写完，没感觉到有什么问题，就是拿到数据后，然后去调用一些函数，通知它们数据来了。但是过了一段时间，同事需要添加一个新的提示框模块。我就得翻几个月前写的代码，添加上新的模块。并且，我之前写的模块也不能随意改名字，一改就要改两个地方。所以模块耦合性太高，影响了编码的效率和增加了维护的难度。发布-订阅模式就可以解决这个问题。

---

## 发布-订阅模式

定义：发布-订阅模式也是观察者模式，它定义一个对象间的一种对多的依赖关系，当一个对象的状态发生改变时，所有依赖它的对象都将得到通知（是不是感觉很晦涩，看下代码，再回过头读读这句话）。

基础版例子：

```
var event = {
    clientList: [],
    listen: function (key, fn) {
        if (!this.clientList[key]) {
            this.clientList[key] = [];
        }
        this.clientList[key].push(fn); // 订阅的消息添加进缓存列表
    },
    trigger: function () {
        var key = Array.prototype.shift.call(arguments),
            fns = this.clientList[key];
        if (!fns || fns.length === 0) { // 如果没有绑定对应的消息
            return false;
        }
        for (var i = 0, fn; fn = fns[i++];) {
            fn.apply(this, arguments); // arguments 是 trigger 时带上的参数
        }
    }
};
//订阅
event.listen( 'apple', function( price ){
    console.log( 'apple = ' + price );
});
//订阅
event.listen( 'orange', function( price ){
    console.log( 'orange = ' + price );
});
//发布
event.trigger( 'apple', 2000000 );
//发布
event.trigger( 'orange', 3000000 );
```

看了代码后，逻辑很简单，就是把要通知的函数都推进一个数组，然后在触发的时候遍历调用数组存储的函数。这是最基本的版的发布订阅模式。

## 取消订阅事件

有时候，也需要取消订阅的订阅的事件。逻辑很简单，把存储在消息列表的函数，删掉就好了。代码如下：

```
vent.remove = function (key, fn) {
    var fns = this.clientList[key];
    if (!fns) { // 如果 key 对应的消息没有被人订阅，则直接返回
        return false;
    }
    if (!fn) { // 如果没有传入具体的回调函数，表示需要取消 key 对应消息的所有订阅
        fns && (fns.length = 0);
    } else {
        for (var l = fns.length - 1; l >= 0; l--) { // 反向遍历订阅的回调函数列表
            var _fn = fns[l];
            if (_fn === fn) {
                fns.splice(l, 1); // 删除订阅者的回调函数
            }
        }
    }
};
```

## 缺点

发布-订阅模式虽然可以弱化对象之间的联系，但如果过度使用的话，对象和对象之间的必要联
系也将被深埋在背后，会导致程序难以跟踪维护和理解。

如果给模块都加上发布订阅的功能，会浪费资源。还存在一定的耦合性，订阅者需要知道发布者的名字。

## 总结

回到最初的问题，模块之间的通信是我一直头疼的问题。尝试了很多的方法，都不能很好的处理模块之间的通信。当时不论怎么写，模块之间的耦合都会影响到编程。到最后，也是实现了一个类似于发布-订阅模式的方法。早知道多看书，这样可以节省很多时间。


