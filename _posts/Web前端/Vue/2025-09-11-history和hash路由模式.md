---
title: history和hash路由模式
date: 2025-09-11
description: history和hash路由模式
tags: [Vue]
categories: [Web前端, Vue]
---
# history和hash路由模式

### 一：什么是路由？前端有哪些路由？他们有哪些特性？

路由是根据不同的url地址来显示不同的页面或内容的功能，这个概念很早是由后端提出的。\ 后端之前是这么做的，当我们访问 http://xxx.abc.com/xx 的时候，大致流程可以想象成这样的：



1. 浏览器向服务器发出请求。\ 2. 服务器监听到80端口，如果有请求过来，那么就解析url地址。\ 3. 服务器根据客户端的路由配置，然后就返回相应的信息(比如html字符串、json数据或图片等)。\ 4. 浏览器根据数据包的 Content-Type来决定如何解析数据。



如上就是后端路由最初始的实现方式，那么既然有后端路由，那为什么还需要我们前端路由呢？后端路由有一个很大的缺点就是每次路由切换的时候都需要去刷新页面，然后发出ajax请求，然后将请求数据返回回来，那么这样每次路由切换都要刷新页面对于用户体验来说就不好了。因此为了提升用户体验，我们前端路由就这样产生了。它就可以解决浏览器不会重新刷新了。



在理解路由之前，我们下面看下History API有哪些方法：\ DOM window对象通过history对象提供了对当前会话浏览历史的访问，在html4中有如下方法：



<code>**window.history.length**</code> 返回当前会话浏览过的页面数量。\ <code>**window.history.go(?delta)**</code>** **接收一个整数作为参数，按照当前页面在会话浏览历史记录中的位置进行移动。如果参数为0、undefined、null、false 将刷新页面，相当于执行window.location.reload()方法。如果参数大于浏览器浏览的数量，或小于浏览前的数量的话，什么都不会做。\ <code>**window.history.back()**</code> 移动到上一页。相当于点击浏览器的后退按钮，等价于 window.history.go(-1);\ <code>**window.history.forward()**</code> 移动到下一页，相当于点击浏览器的前进按钮，等价于window.history.go(1).

![1638238596953-6d3b3149-e15f-49e5-b64a-bd7551ee1399.png](/assets/img/posts/Web前端/Vue/1638238596953-6d3b3149-e15f-49e5-b64a-bd7551ee1399-324426.png)



在html5中，History API 新增了操作会话浏览历史记录的功能。如下新增的几个方法：\ window.history.state. 该参数是只读的，表示与会话浏览历史的当前记录相关联的状态对象。如下图所示：

![1638238596882-fb815388-20c4-4c20-9996-b055995e359a.png](/assets/img/posts/Web前端/Vue/1638238596882-fb815388-20c4-4c20-9996-b055995e359a-078853.png)

**window.history.pushState(data, title, ?url):** 在会话浏览历史记录中添加一条记录。

**window.history.replaceState(data, title, ?url):** 该方法用法和history.pushState方法类似，但是该方法的含义是将修改会话浏览历史的当前记录，而不是新增一条记录。也就是说把当前的浏览地址换成 replaceState之后的地址，但是浏览历史记录的总长度并没有新增。

注意：执行上面两种方法后，url地址会发生改变。但是不会刷新页面。因此有了这些基本知识后，我们再来看下前端路由。

那么前端路由也有2种模式，第一种是hash模式，第二种是history模式。我们来分别看下这两种知识点及区别如下：

****

### 1. hash模式

hash路由模式是这样的：http://xxx.abc.com/#/xx。 有带#号，后面就是hash值的变化。改变后面的hash值，它不会向服务器发出请求，因此也就不会刷新页面。并且每次hash值发生改变的时候，会触发hashchange事件。因此我们可以通过监听该事件，来知道hash值发生了哪些变化。比如我们可以如下简单的监听：

```javascript
function hashAndUpdate () {
   // todo 匹配 hash 做 dom 更新操作
}

window.addEventListener('hashchange', hashAndUpdate);
```



****我们先来了解下location有哪些属性，如下：

```javascript
// 完整的url
location.href  

// 当前URL的协议，包括 :; 比如 https:     
location.protocol   

/* 主机名和端口号，如果端口号是80(http)或443(https), 那就会省略端口号，比兔 www.baidu.com:8080 */
location.host  

// 主机名：比如：www.baidu.com
location.hostname
  
// 端口号；比如8080
location.port

// url的路径部分，从 / 开始; 比如 https://www.baidu.com/s?ie=utf-8，那么 pathname = '/s'了
location.pathname

// 查询参数，从?开始；比如 https://www.baidu.com/s?ie=utf-8 那么 search = '?ie=utf-8'
location.search

// hash是页面中的一个片段，从 # 开始的，比如 https://www.baidu.com/#/a/b 那么返回值就是："#/a/b"
location.hash 
```

**location.href**

****

我们通过改变location.href来改变对应的url，看看是否会刷新页面，我们做如下测试可以看到，使用location.href 改变url后并不会刷新页面，如下代码在控制台中演示：

![1638238596909-007d3bce-7a76-4fcf-b63c-e5e908086fcf.png](/assets/img/posts/Web前端/Vue/1638238596909-007d3bce-7a76-4fcf-b63c-e5e908086fcf-403251.png)

****

**location.hash**

****

改变hash不会触发页面跳转，因为hash链接是当前页面中的某个片段，所以如果hash有变化，那么页面将会滚动到hash所连接的位置。但是页面中如果不存在hash对应的片段，则没有任何效果。比如 a链接。这和 window.history.pushState方法类似，都是不刷新页面的情况下更改url。如下也可以看到操作并没有刷新url，如下演示：\ ![1638238597216-bf707079-71b5-4d7d-acc7-721d19b8efa3.png](/assets/img/posts/Web前端/Vue/1638238597216-bf707079-71b5-4d7d-acc7-721d19b8efa3-494932.png)

**hash 和 pushState 对比有如下缺点：**

1. hash只能修改url的片段标识符的部分。并且必须从#号开始，但是pushState且能修改路径、查询参数和片段标识符。pushState比hash更符合前端路由的访问方式，更加优雅(因为不带#号)。

2. hash必须和原先的值不同，才能新增会话浏览历史的记录，但是pushState可以新增相同的url的记录，如下所示：

![1638238597229-f6e21618-44b4-461c-a03f-a0232ec7a568.png](/assets/img/posts/Web前端/Vue/1638238597229-f6e21618-44b4-461c-a03f-a0232ec7a568-523996.png)

****

**1.1 使用hashchange事件来监听url hash的改变**

我们来演示下，我们使用node启动一个服务，然后有一个index.html页面，该页面引入了一个js文件，该js文件有如下js代码：

```javascript
window.addEventListener('hashchange', function(e) {
  console.log(e)
});
```

如上代码就是监听hash值发生变化的事件，然后我们访问该index.html页面后，然后在控制台中，做如下操作，如下图演示：

![1638238597421-bc289bad-97a8-4b03-96ba-1831c1adf99f.png](/assets/img/posts/Web前端/Vue/1638238597421-bc289bad-97a8-4b03-96ba-1831c1adf99f-228663.png)

如上可以看到；不管我们是通过location接口直接改变hash值，还是我们通过history直接前进或后退操作(改变hash变化)，我们都可以看到都能通过 hashchange该事件进行监听到url hash的改变。并且不会刷新页面。

****

### 2. history模式

HTML5的History API为浏览器的全局history对象增加了该扩展方法。它是一个浏览器的一个接口，在window对象中提供了onpopstate事件来监听历史栈的改变，只要历史栈有信息发生改变的话，就会触发该事件。提供了如下事件：

```javascript
window.addEventListener('popstate', function(e) {
  console.log(e)
});
```

history提供了两个操作历史栈的API: **history.pushState 和 history.replaceState**

**history.pushState(data\[,title]\[,url]);** // 向历史记录中追加一条记录

**history.replaceState(data\[,title]\[,url]);** // 替换当前页在历史记录中的信息。

如上html5中新增了上面这两个方法，该两个方法也可以改变url，页面也不会重新刷新。下面我们也可以来做个demo，来监听下popstate事件，现在在我js里面放入如下js代码:

```javascript
window.addEventListener('popstate', function(e) {
  console.log(e)
});
```

然后我们访问页面，如下所示：

![1638238597390-f461cf4d-2cb2-43e2-9203-a87cceb8cbf3.png](/assets/img/posts/Web前端/Vue/1638238597390-f461cf4d-2cb2-43e2-9203-a87cceb8cbf3-130039.png)

如上图所示，我们使用location.hash, history.go(-1), history.pushState 等方法操作都会触发 popstate 事件，并且浏览器的url地址也会跟着改变。只会改变url地址，且不会重新刷新页面。

****

**hash模式的特点：**

hash模式在浏览器地址栏中url有#号这样的，比如(http://localhost:3001/#/a). # 后面的内容不会传给服务端，也就是说不会重新刷新页面。并且路由切换的时候也不会重新加载页面。

****

**history模式的特点：**

浏览器地址没有#， 比如(http://localhost:3001/a); 它也一样不会刷新页面的。但是url地址会改变。


