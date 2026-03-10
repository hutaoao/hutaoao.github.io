---
title: react-router-domv6的类组件使用路由的参数
date: 2025-08-07
description: react-router-dom v6 的类组件使用路由的参数
tags: [react]
categories: [Web前端, React]
---
# react-router-dom v6 的类组件使用路由的参数

**react-router从v5升级到v6之后做了非常大的改动**

****

**在类组件中，发现props是一个空对象！！！** 



+ 编程式路由导航，在非受控组件中可以使用useNavigate这个钩子进行导航，而在类组件中无能为力，只能想办法使用<Navigate>这个标签，非常的麻烦，可以看看这篇文章：[https://www.jianshu.com/p/5bdfd2fac2cd](https://www.jianshu.com/p/5bdfd2fac2cd)
+ 获取路由参数 ，在以往的react-router-dom版本中，路由的三个参数location、history、match都是直接挂载到组件的props身上，即使组件不是路由组件，也可以使用withRouter高阶组件对普通组件进行增强，也可以将这三个参数带到props身上。在v6版本中withRouter直接被移除。怎么办？



自己编写高阶组件withRouter从而实现这一需求，可以看看这篇文章中的回答：[https://cloud.tencent.com/developer/ask/sof/296970](https://cloud.tencent.com/developer/ask/sof/296970)



```javascript
export function withRouter( Child ) {
  return ( props ) => {
    const location = useLocation();
    const navigate = useNavigate();
    return <Child { ...props } navigate={ navigate } location={ location } />;
  }
}

```



```javascript
import React from "react";
import { NavigateFunction, useLocation, useNavigate, useParams } from "react-router";

export interface RoutedProps<Params = any, State = any> {
    location: State;
    navigate: NavigateFunction;
    params: Params;
}


export function withRouter<P extends RoutedProps>( Child: React.ComponentClass<P> ) {
    return ( props: Omit<P, keyof RoutedProps> ) => {
        const location = useLocation();
        const navigate = useNavigate();
        const params = useParams();
        return <Child { ...props as P } navigate={ navigate } location={ location } params={ params }/>;
    }
}

```



使用：

```javascript
import React from 'react'
import { withRouter, RoutedProps } from './WithRouter'

class Result extends React.Component<RoutedProps> {
  componentDidMount (): any {
    console.log(this.props)
  }

  render (): any {
    return <div>111</div>
  }
}

export default withRouter(Result)

```

控制台输出：

![1662621214643-9a8496c1-01f3-4713-ac68-8c92b3ba0a90.png](/assets/img/posts/Web前端/React/1662621214643-9a8496c1-01f3-4713-ac68-8c92b3ba0a90-600797.png)



