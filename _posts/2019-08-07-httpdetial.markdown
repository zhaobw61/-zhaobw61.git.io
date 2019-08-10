---
layout:       post
title:        "HTTP协议原理"
subtitle:     "HTTP各种特性总览"
date:         2019-08-07 21:00:00
author:       "boowen"
header-img:   "img/post-bg-pubsub.png"
header-mask:  0.3
tags:
    - 前端
    - HTTP
---
>我对于HTTP一直都是模糊状态，只了解一些基本的知识。今天决定深入学习下。

## 学习目的

能够和后端高效的沟通（其实是想甩锅啦）

## CORS跨域请求

### 跨域

请求已经发送了，内容已经返回了，但是浏览器给禁止了。

- 在服务端添加

`
res.writeHead(200,{
    'Access-Control-Allow-Origin': '*'
})
`

- JSONP

> 因为link标签、img标签、js标签请求没有同域的限制，所以可以跨域。所以可以在js里添加跨域请求。

### CORS预请求

- 允许的方法：GET、HEAD、POST

- 允许Content-Type：text/plain、multipart/form-data application/x-www-form-urlencoded

- 自定义头部：Access-Control-Allow-Headers:''

- 自定义请求方法：Access-Control-Allow-Methods:'POST, PUT, Delete'

- 允许自定义的请求时间：Access-Control-Max-Age:'1000' 


## 缓存 Cache-Control

### 可缓存性

- public：任何地方可以缓存

- private：发起请求的地方进行缓存

- no-cache：任何地方都不缓存，每次都要经过服务器的确认 设置状态码304可以直接读缓存

### 到期

- max-age = <time>

- s-maxage = <time>

- max-stale = <time>

### 重新验证

- must-revalidate

- proxy-revalidate

### 其他

- no-store：没有任何缓存 

- no-transform

## 缓存验证

- Last-Modified：上次修改时间

- Etag：数据签名

## Cookie和Session

### Cookie属性

- max-age和expires设置过期时间

- Secure只在https的时候发送

- httpOnly无法通过document.cookie访问

- 如果没有设置时间，在关闭浏览器后就会失效。

## HTTP长连接

### 长连接

- 一个TCP连接可以发多个请求

- 在返回头部设置：connection:close/keep-live

- 谷歌浏览器最多保持六个TCP连接

## 数据协商


## Redirect

> 资源迁移，服务器告诉新地址，浏览器重新请求新的地址。状态码需要设置为302

- 302：临时变更。301：是永久变更，浏览器会进行缓存。

## Content-Security-Policy

作用：
- 限制资源获取
- 报告资源获取越权

限制方式：
- default-src:限制全局
- 定制资源类型
- Content-Security-Policy:'report ' 向服务器汇报资源请求 

## https

### 加密

- 私钥：

- 公钥：











