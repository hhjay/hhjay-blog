---
title: http请求的流程及状态码含义
date: 2018-04-17 10:38:29
tags: http
---

## 请求流程
- 域名解析
    - 浏览器搜索浏览器自身的DNS缓存
        - 若没有找打缓存则会查看系统的DNS缓存
        - 若找到且没有过期则停止搜索解析
        - 若本机没有找到DNS缓存，则浏览器会发起一个DNS的系统调用，会像本地配置的DNS服务器发起域名解析
        - 找到Ip地址
- 发起Tcp三次握手
    - UA向服务器的web程序(httped、nginx)发起Tcp的连接请求
    - 连接请求到达服务端后
    - 进入到内核的Tcp/Ip协议栈，识别连接请求、解封包
    - 到达Web程序，最终建立了Tcp/Ip的连接
- 建立Tcp连接后发起http请求，浏览器得到html代码
- 浏览器解析html代码，并请求html代码中的资源（例如js、css、img...）
- 浏览器对页面进行渲染（渲染流程参见其他页面）

## 304怎么来的
- 客户端在请求一个文件的时候，发现缓存的文件有Last Modified，则在请求中会包含If-Modified-Since，这个时间就是缓存文件的Last-Modified。因此，若请求中包含If-Modified-Since则说明有缓存在客户端，服务器只需要判断这个时间和当前请求文件的修改时间就可以确定是返回200还是304
- 对于静态文件，服务端会自动比较Last-Modified和If-Last-Modified
- 200 OK (FROM CACHE) 与 304 NOT MODIFIED
    - 200 OK (from cache)  是浏览器没有跟服务器确认，直接用了浏览器缓存；而 304 Not Modified 是浏览器和服务器多确认了一次缓存有效性，再用的缓存。

## 常规操作
- F5 刷新，浏览器会设置max-age=0，跳过强缓存判断，会进行协商缓存判断
- Ctrl + F5, 跳过协商缓存与强缓存，直接从服务器拉取资源
- 地址栏访问，链接跳转是正常用户行为，将会触发浏览器缓存机制

## 状态码
- 1xx 
    - 临时响应：代表请求已被接受，需要继续处理
    - 100 服务器已接收到请求头，并且客户端继续发生请求主体；或者请求已经完成，忽略这个响应
    - 101 服务器已经理解了客户端的请求，并将通过Upgrade消息头通知客户端采用不同的协议完成这个请求，比如http1到http2
    - 102 请求可能包含许多涉及文件操作的子请求，需要很长时间才能完成请求
- 2xx 
    - 成功：表示请求已经被服务器接收、理解
    - 200 请求已成功，请求所希望的响应头或数据体将响应返回
    - 201 请求已经被实现，且有一个新的资源已经依据请求的需要创建，其URI已经随Location头信息返回
    - 202 服务器已接收请求，但尚未处理
    - 203 服务器是一个转换代理服务器
    - 204 服务器成功处理了请求，没有返回任何内容
- 3xx
    - 重定向：后续的请求地址在本次的Location中指明
    - 301 请求的资源已永久的移动到新位置
    - 302 请求的资源临时指向新的位置
    - 304 资源未被修改
- 4xx
    - 客户端错误：客户端发生了错误，妨碍了客户端的处理
    - 400 参数传错（格式错误、大小出错、无效的请求消息、欺骗路由...） 
    - 403 Forbidden，服务器已经理解请求，但是拒绝执行
    - 404 请求的路径错误
    - 405 请求方法错误，如get的用post请求
    - 414 请求的URI过长
- 5xx
    - 服务器错误，表示服务器无法完成有效的请求
    - 500 Internal Server Error，服务器遇到错误，无法响应
    - 502 Bad Gateway，网关或代理的服务器无效响应
    - 503 Service Unavailable，服务器维护或过载无法完成请求
    - 504 Gateway Timeout，网关或代理的服务器响应超时

## 参考链接
- [简书.请求流程](https://www.jianshu.com/p/fbe0e9fa45a6)
- [博客园.紫云飞](http://www.cnblogs.com/ziyunfei/archive/2012/11/16/2772729.htm)
- [维基百科.http状态码](https://zh.wikipedia.org/wiki/HTTP%E7%8A%B6%E6%80%81%E7%A0%81)
- [200和304](https://div.io/topic/854)
- [juejin.预加载和强缓存](https://juejin.im/post/5b17e7f5e51d4506af2e8e42)
- [强缓存和协商缓存图](https://user-gold-cdn.xitu.io/2018/6/7/163d60a0d0e7bf24?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)