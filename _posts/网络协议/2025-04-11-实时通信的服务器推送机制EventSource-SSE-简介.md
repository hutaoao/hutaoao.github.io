---
title: 实时通信的服务器推送机制EventSource-SSE-简介
date: 2025-04-11
description: 实时通信的服务器推送机制 EventSource(SSE) 简介
tags: [网络协议]
categories: [网络协议]
---
# 实时通信的服务器推送机制 EventSource(SSE) 简介

### 简介

不知道大家有没有见过 <code><font style="color:#DF2A3F;">Content-Type:text/event-stream</font></code> 的请求头，这是 <code><font style="color:#DF2A3F;">HTML5</font></code> 中的 <code><font style="color:#DF2A3F;">EventSource</font></code> 是一项强大的 <code><font style="color:#DF2A3F;">API</font></code>，通过服务器推送实现实时通信。

与 <code><font style="color:#DF2A3F;">WebSocket</font></code> 相比，<code><font style="color:#DF2A3F;">EventSource</font></code> 提供了一种简单而可靠的单向通信机制（服务器->客户端），实现简单，适用于许多实时应用场景。

本文将介绍 <code><font style="color:#DF2A3F;">EventSource</font></code> 的简单使用、与 <code><font style="color:#DF2A3F;">WebSocket</font></code> 的对比以及其优缺点，最后对其进行总结。

### 概念

<code><font style="color:#DF2A3F;">EventSource</font></code>是<code><font style="color:#DF2A3F;">HTML5</font></code>中的一种新的<code><font style="color:#DF2A3F;">API</font></code>，基于 <code><font style="color:#DF2A3F;">HTTP</font></code> 协议，用来实现服务器端向客户端推送事件、用于在客户端和服务器之间建立持久的、单向的通信连接。相比于常规的轮询方式，<code><font style="color:#DF2A3F;">EventSource</font></code>可以实现更加\*\*<font style="color:#DF2A3F;">高效</font>**、**<font style="color:#DF2A3F;">低延迟</font>\*\*的数据传输。

它的基本使用方式是：首先在客户端创建一个<code><font style="color:#DF2A3F;">EventSource</font></code>对象，然后向指定的服务器端<code><font style="color:#DF2A3F;">URL</font></code>发送一个<code><font style="color:#DF2A3F;">HTTP</font></code>请求。此时，服务器端需要支持<code><font style="color:#DF2A3F;">EventStream</font></code>格式，即<code><font style="color:#DF2A3F;">Content-Type</font></code>为<code><font style="color:#DF2A3F;">text/event-stream</font></code>。一旦客户端收到了这个请求的响应，它就会开始<font style="color:#DF2A3F;">监听</font>服务器端的事件，并将事件流动态地展现在网页中。<code><font style="color:#DF2A3F;">EventSource</font></code> 也叫作 <code><font style="color:#DF2A3F;">SSE(server-sent-event)</font></code>。

### 特性

1. 实时性。<code><font style="color:#DF2A3F;">EventSource</font></code>能够实现实时地数据传输，可以在服务器端有新事件时立即向客户端推送，并自动进行展示。
2. 低延迟。由于<code><font style="color:#DF2A3F;">EventSource</font></code>采用长连接的方式进行传输，相比于普通的轮询方式，它能够\*\*<font style="color:#DF2A3F;">更加高效地传输数据</font>\*\*。
3. 易用性。<code><font style="color:#DF2A3F;">EventSource</font></code>是一种非常易用的API，只需要在客户端创建一个EventSource对象，指定服务器端的URL，即可进行监听并展示事件。
4. 兼容性。<code><font style="color:#DF2A3F;">EventSource</font></code>能够同时兼容<code><font style="color:#DF2A3F;">WebSocket</font></code>和长轮询等方式，具备\*\*<font style="color:#DF2A3F;">很好的兼容性</font>\*\*，可以在各种不同的场景下使用。

### <font style="color:rgb(79, 79, 79);">应用场景</font>

<code><font style="color:#DF2A3F;">EventSource</font></code>主要用来实现服务器端向客户端实时推送事件，它在Web应用中的应用场景非常广泛。下面列举几个具体的应用场景：

1. **即时聊天**。在即时聊天应用中，EventSource可以实现实时向客户端推送新消息，从而保证聊天效果的实时性和流畅性。
2. **数据监控**。在数据监控应用中，EventSource可以实时向客户端推送最新的监控数据，从而让用户及时掌握系统状态。
3. **消息提示**。在消息提示应用中，EventSource可以实时向客户端推送最新的通知信息，让用户不会错过任何重要消息。
4. **广告推送**。在广告推送应用中，EventSource可以实时向客户端推送最新的广告信息，从而提高广告的投放效果。

### <font style="color:rgb(79, 79, 79);">优缺点</font>

#### 优点

（1）实时展示：EventSource能够实现实时展示服务器端的事件，相比于常规的轮询方式，它能够更加高效、低延迟的展示数据。

（2）易用性：EventSource是一种非常易用的API，只需要在客户端创建一个EventSource对象，指定服务器端的URL，即可进行监听并展示事件。

（3）兼容性良好：EventSource能够同时兼容WebSocket和长轮询等方式，具备很好的兼容性，可以在各种不同的场景下使用。

（4）网络带宽节省：EventSource采用长连接的方式进行数据传输，相比于普通的轮询方式，能够节省大量的网络带宽。

#### 缺点

（1）一次性消息：EventSource只能一次性地向客户端推送一条消息，而不能像WebSocket那样实现双向通讯。

（2）不支持二进制数据传输：EventSource只能传输文本数据，不能传输二进制数据，这在某些场景下可能存在一定的局限性。

（3）不支持重连：如果网络连接不稳定，或者服务器端关闭EventStream连接，客户端需要重新连接才能继续监听事件。

### <font style="color:rgb(79, 79, 79);">使用</font>

<font style="color:rgb(77, 77, 77);">使用EventSource也比较简单，只需要创建一个EventSource对象并指定服务器端的URL即可。</font>

```javascript
var eventSource = new EventSource("/events");
eventSource.onmessage = function(event) {
  console.log("Received event: " + event.data);
};
```

在这个示例中，创建了一个EventSource对象，并指定服务器端的URL为"/events"。同时，还注册了一个onmessage事件回调函数，在每次收到服务器端推送的事件时，会打印出事件数据。

在服务器端，需要确保能够接收和处理EventStream格式的HTTP请求。下面是一个简单的Node.js的Express应用示例：

```javascript
const express = require('express');
const app = express();

app.get('/events', function(req, res) {
  res.set({
    'Content-Type': 'text/event-stream',
    'Cache-Control': 'no-cache',
    'Connection': 'keep-alive'
  });

  setInterval(function() {
    res.write('data: ' + new Date().toISOString() + '\n\n');
  }, 1000);
});

app.listen(3000, function() {
  console.log('Example app listening on port 3000!');
});
```

在这个示例中，创建了一个Express应用，并通过路由"/events"来处理EventSource请求。其中，将响应的Content-Type设置为text/event-stream，表示返回的数据格式为EventStream。同时，通过设置Cache-Control和Connection实现长连接的功能。在每秒钟向客户端推送一个带时间戳的事件。


> 更新: 2023-09-06 10:09:02  
> 原文: <https://www.yuque.com/hutaoao/blog/pxy791b6fi92mf28>