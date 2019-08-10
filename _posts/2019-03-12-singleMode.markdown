---
layout:       post
title:        "常用设计模式"
subtitle:     "单例模式"
date:         2019-03-12 21:00:00
author:       "boowen"
header-img:   "img/post-bg-pubsub.png"
header-mask:  0.3
tags:
    - 前端
    - JS
    - 设计模式
---
>其实设计模式并没有那么高深，它只是一种写代码的套路。或许在写代码的时候，你已经用了某种设计模式，但是并不知道它是有名字的。现在总结下常用的设计模式。以后方便装逼。

## 学习目的

>学习一样东西的目的，自然是要解决实际的问题。

接到一个需求，我需要写出一个模态框，模态框里的类型很多。点击按钮，弹出一个对话框。我当时的策略是写了多个对应的构造函数，每次需要弹出一个模态框的时候，就new一个对象出来。模态框的对应的HTML就放在页面里。但是这样写的话有缺点，如果其他的页面需要模态框，又得复制一遍模态框的HTML。而且有很多不同类型的模态框，大概有30来个，意味着同事使用的时候要问我对应类型模态框的名字，如果添加一个。所以我就把所有的创建的模态框的方法，都由一个构造函数createModule来管理。使用者只需要在创建的时候，传入对应的类型，就可以创建相应的类型的模态框。createModule函数内部用一个对象存储了映射了对应的模态框的构造的函数的关系。在每次创建的时候，都会检查是否重复创建。写到这里，有一点单例模式的影子了。但是还有一些缺陷，例如违背了单一职责的原则。


## 单例模式

定义：保证一个类仅有一个实例，并提供一个访问它的全局访问点。


单例模式很好理解，就是保证全局只有这一个对象，但是怎么提供这个对象，就有很多细节了，但是关键点就一个，就是用一个变量来标志是否已经为当前的类创建过对象。应用场景：多应用于弹窗，线程池、全局缓存、浏览器中的 window 对象等。

## 基础版的单例模式

超级简单版的单例模式，就是用一个变量判断是否创建了实例。

```
var Singleton = function (name) {
    this.name = name;
    this.instance = null;
};
Singleton.prototype.getName = function () {
    alert(this.name);
};
Singleton.getInstance = function (name) {
    if (!this.instance) {
        this.instance = new Singleton(name);
    }
    return this.instance;
};

var a = Singleton.getInstance( 'sven1' );
var b = Singleton.getInstance( 'sven2' );
alert ( a === b ); // true

```

代码里用this.instance来判断当前是否已经创建过实例。但是这个基础版有个缺点：获得实例是通过Singleton.getInstance方法来获取实例的，和我们平时new的这个方法有点不同。因此继续改进。

## 透明的单例模式
 
现在需要的单例模式是通过new的方法来创建实例。代码如下：

```
var CreateDiv = (function () {
    var instance;
    var CreateDiv = function (html) {
        if (instance) {
            return instance;
        }
        this.html = html;
        this.init();
        return instance = this;
    };
    CreateDiv.prototype.init = function () {
        var div = document.createElement('div');
        div.innerHTML = this.html;
        document.body.appendChild(div);
    };
    return CreateDiv;
})();
var a = new CreateDiv('sven1');
var b = new CreateDiv('sven2');
```

这次通过闭包，解决了之前的问题。但是有两个小问题，第一阅代码读起来不是清晰，这个构造函数看起来很奇怪，第二违背了单一职责原则，在CreateDiv这个函数里，它负责了两件事，第一事判断是不是单例，第二事创建对象和初始化。所以又出现新的问题了，需要去解决它。

## 用代理模式实现的单例模式

用代理模式就可以解决上面面临的问题。代码如下：

```
var CreateDiv = function (html) {
    this.html = html;
    this.init();
};
CreateDiv.prototype.init = function () {
    var div = document.createElement('div');
    div.innerHTML = this.html;
    document.body.appendChild(div);
};
var ProxySingletonCreateDiv = (function () {
    var instance;
    return function (html) {
        if (!instance) {
            instance = new CreateDiv(html);
        }
        return instance;
    }
})();
var a = new ProxySingletonCreateDiv('sven1');
var b = new ProxySingletonCreateDiv('sven2');
```
ProxySingletonCreateDiv负责里判断是否单一的职责，解耦了对象的初始化和判断是否是单例。

## 真的有必要模拟类吗？

javaScript是一种没有类的语言，我们用原型链模拟了类的继承和派生、多态。模拟类的存在还有很多缺点，例如显示伪多态。这都给开发人员留下了很多陷阱。不过ES6新添了Class。在JS里创建对象非常简单。

## 惰性单例

> 提前创建好单例，有可能出现没有使用的情况，就会浪费资源。因此在需要的时候创建

## 总结

单例模式是一种简单但非常实
用的模式，特别是惰性单例技术，在合适的时候才创建对象，并且只创建唯一的一个。更奇妙的是，创建对象和管理单例的职责被分布在两个不同的方法中，这两个方法组合起来才具有单例模式的威力。


