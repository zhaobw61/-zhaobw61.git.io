---
layout:       post
title:        "常用设计模式"
subtitle:     "策略模式"
date:         2019-08-29 21:00:00
author:       "boowen"
header-img:   "img/post-bg-pubsub.png"
header-mask:  0.3
tags:
    - 前端
    - JS
    - 设计模式
---
>其实设计模式并没有那么高深，它只是一种写代码的套路。或许在写代码的时候，你已经用了某种设计模式，但是并不知道它是有名字的。

## 学习目的

>学习一样东西的目的，自然是要解决实际的问题。



## 用策略模式

定义：定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换。在实际开发中，我们通常会把算法的含义扩散开来，使策略模式也可以用来封装一系列的“业务规则”。只要这些业务规则指向的目标一致，并且可以被替换使用，我们就可以用策略模式来封装它们。

装饰者也是包装器，由于JS的特殊性。我们可以直接修改写修改对象或者方法。我个人感觉就是重新赋值了对象的属性值。

## 装饰函数

因为初始的对象功能不够用，所以需要去添加对象，但是我并不喜欢这么做，因为需要去阅读别人的代码，而且，每个面对的业务情况也不一样，所以古老的代码和别人的代码，最好不要碰。所以可以用哪个装饰者函数来在不修改源码的情况下，新添功能。

```
window.onload = function(){
    alert (1);
}
var _onload = window.onload || function(){};
    window.onload = function(){
    _onload();
    alert (2);
}
```

但是这个方法有缺点：

-  必须维护 _onload 这个中间变量，虽然看起来并不起眼，但如果函数的装饰链较长，或者需要装饰的函数变多，这些中间变量的数量也会越来越多。

- this可能被劫持的问题

出现了新的问题，所以需要去解决它。

### AOP 装饰函数

代码实现

```
Function.prototype.before = function( beforefn ){
    var __self = this;
    return function(){
        beforefn.apply( this, arguments );
        return __self.apply( this, arguments );
    }
}
Function.prototype.after = function( afterfn ){
    var __self = this;
    return function(){
        var ret = __self.apply( this, arguments );
        afterfn.apply( this, arguments );
        return ret;
    }
};
```

Function.prototype.before 接受一个函数当作参数，这个函数即为新添加的函数。this就是调用before函数的函数，也就是原函数。然后返回一个新的函数。这个新的函数就包含了新的函数和旧的函数的功能。

## 总结
这种模式在实际开发中非常有用，它在框架开发中也十分有用。作为框架作者，希望框架里的函数提供的是一些稳定而方便移植的功能，那些个性化的功能可以在框架之外动态装饰上去，这可以避免为了让框架拥有更多的功能，而去使用一些 if 、 else 语句预测用户的实际需要。