---
title: react-router-dom-v5-x-v6-x-路由传参的方式
date: 2025-08-06
description: react-router-dom(v5.x & v6.x)路由传参的方式
tags: [react]
categories: [Web前端, React]
---
# react-router-dom(v5.x & v6.x)路由传参的方式
<Link to='/demo/test/tom/18'}>详情</Link>
{% raw %}
//或 <Link to={{ pathname:'/demo/test/tom/18' }}>详情</Link> 
{% endraw %}
```

注册路由(声明接收)：

```jsx
<Route path="/demo/test/:name/:age" component={Test}/> 
```

接收参数：

```jsx
this.props.match.params
```



#### 2.search参数
路由链接(携带参数)：

```jsx
<Link to='/demo/test?name=tom&age=18'}>详情</Link> 
```

注册路由(无需声明，正常注册即可)：

```jsx
<Route path="/demo/test" component={Test}/> 
```

接收参数：

```jsx
this.props.location.search
```

备注：获取到的search是urlencoded编码字符串(例如: ?id=10&name=zhangsan)，需要借助query-string解析参数成对象

#### 3.state参数
路由链接(携带参数)：

```jsx
{% raw %}
<Link to={{pathname:'/demo/test',state:{name:'tom',age:18}}}>详情</Link> 
{% endraw %}
```

注册路由(无需声明，正常注册即可)：

```jsx
<Route path="/demo/test" component={Test}/> 
```

 接收参数：

```jsx
this.props.location.state
```

> **特点：**  
1、BrowserRouter(history)模式下，刷新页面参数不消失，参数不会在地址栏显示，因为state保存在history对象中  
2、HashRouter(hash)模式下，刷新页面参数消失！！！参数不会在地址栏显示

### 路由传值的三种方式（v6.x）
#### 1.params参数
路由链接(携带参数)：

```jsx
{% raw %}
<Link to={{ pathname:`/b/child1/${id}/${title}` }}>Child1</Link> 
{% endraw %}
//或 <Link to={/b/child1/${id}/${title}}>Child1
```

注册路由(声明接收)：

```jsx
<Route path="/b/child1/:id/:title" component={Test}/> 
```

接收参数：

```jsx
import { useParams } from "react-router-dom";
const params = useParams(); //params参数 => {id: "01", title: "消息1"} 
```

#### 2.search参数
路由链接(携带参数)：

```jsx
<Link className="nav" to={`/b/child2?age=20&name=zhangsan`}>Child2</Link> 
```

注册路由(无需声明，正常注册即可)：

```jsx
<Route path="/b/child2" component={Test}/> 
```

接收参数方法1：

```jsx
//接收参数方法1：
import { useLocation } from "react-router-dom";
import qs from "query-string";
const { search } = useLocation();
//search参数 => {age: "20", name: "zhangsan"}

//接收参数方法2：
import { useSearchParams } from "react-router-dom";
const [searchParams, setSearchParams] = useSearchParams();
// console.log( searchParams.get("id")); // 12

//备注：获取到的search是urlencoded编码字符串(例如: ?age=20&name=zhangsan)，需要借助query-string解析参数成对象
```

#### 3.state参数
通过Link的state属性传递参数

```jsx
 <Link
   className="nav"
   to={`/b/child2`}
{% raw %}
   state={{ id: 999, name: "i love merlin" }}
{% endraw %}
> Child2</Link> 
```

注册路由(无需声明，正常注册即可)：

```jsx
<Route path="/b/child2" component={Test}/> 
```

接收参数：

```jsx
import { useLocation } from "react-router-dom";
const { state } = useLocation();
//state参数 => {id: 999, name: "我是梅琳"}
```

> **特点：**  
1、BrowserRouter(history)模式下，刷新页面参数不消失，参数不会在地址栏显示，因为state保存在history对象中  
2、HashRouter(hash)模式下，刷新页面参数消失！！！参数不会在地址栏显示



