---
layout: post
title: 跨域 Recap
category:
- cross-domain
---

### 1. 什么是跨域

Same-origin policy 同源策略

> An *origin* is defined by the scheme, host, and port of a URL. Generally speaking, documents retrieved from distinct origins are isolated from each other. For example, if a document retrieved from http://example.com/doc.html tries to access the DOM of a document retrieved from https://example.com/target.html, the user agent will disallow access because the origin of the first document, (http, example.com, 80), does not match the origin of the second document (https, example.com, 443). For networking APIs, the same-origin policy distinguishes between sending and receiving information. Broadly, one origin is permitted to send information to another origin, but one origin is not permitted to receive information from another origin. The prohibition on receiving information is intended to prevent malicious web sites from reading confidential information from other web sites, but also prevents web content from legitimately reading information offered by other web sites.[[1]](http://www.w3.org/Security/wiki/Same_Origin_Policy)

> The same-origin policy also applies to XMLHttpRequests unless the server provides a Access-Control-Allow-Origin (CORS) header. Notably WebSockets are not subject to same-origin policy. [[2]](https://en.wikipedia.org/wiki/Same-origin_policy)

要点：

*	协议、域名、端口定义了一个源。
*	对于网络API，浏览器(user agent)允许一个源往其他源发送消息，但是不允许一个源接受其他源的消息。
*	同源策略适用于AJAX请求，不适用于WebSockets。

比如我在tail.moe:8765运行一个Web Service，在tail.moe:80运行一个Web Site。tail.moe:8765和tail.moe:80不是同一个origin，该Site的JavaScript脚本无法请求Web Service。要突破
同源策略，使得JavaScript脚本可以请求Web Service，需要使用跨域方法(“源”和“域”是一个意思……大概)。

### 2. 常见的跨域方法

#### 2.1 JSONP

> JSONP or "JSON with padding" is a communication technique used in JavaScript programs running in web browsers to request data from a server in a different domain, something prohibited by typical web browsers because of the same-origin policy. JSONP takes advantage of the fact that browsers do not enforce the same-origin policy on <script> tags.
>
> Note that for JSONP to work, a server must know how to reply with JSONP-formatted results. JSONP does not work with JSON-formatted results. The JSONP parameters passed as arguments to a script are defined by the server.

要点：

*	JSONP利用浏览器“不要求\<script\>标签同源”实现跨域。
*	JSONP要求服务器正确响应JSONP请求。
*	!! 只能利用JSONP发送GET方法。

#### 2.2 Cross-origin resource sharing (CORS)

> Cross-origin resource sharing (CORS) is a mechanism that allows many resources (e.g., fonts, JavaScript, etc.) on a web page to be requested from another domain outside the domain from which the resource originated. In particular, JavaScript's AJAX calls can use the XMLHttpRequest mechanism. CORS defines a way in which the browser and the server can interact to determine whether or not to allow the cross-origin request. [[3]](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)

要点：

*	CORS允许跨域获取资源，包括字体、JavaScript脚本、AJAX请求。
*	CORS定义了一种浏览器和服务器交互的方法，以确定是否允许跨域访问。

关于如何开启服务器的CORS，[enable-cors.org](http://enable-cors.org/)上有详尽的描述。只允许GET/POST方法的simple CORS requests比较简单。

### 3 Hacking

流氓会武术，谁也挡不住。1中举的例子，如果Web Site和Web Service都是自己的服务，又懒得开启CORS，可以在Web Site的服务器上使用消息转发，将tail.moe:80/foobar/转发至tail.moe:8765。一秒钟跨域变同域。。。









