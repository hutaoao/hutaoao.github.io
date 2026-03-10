---
title: React-router-domv6-x
date: 2025-07-28
description: React-router-dom v6.x
tags: [React, react]
categories: [Web前端, React]
---
# React-router-dom v6.x

**嵌套路由**

目录结构：

![1647833712980-c2c34d54-f30f-4fbb-9242-1851c0363d43.png](/assets/img/posts/Web前端/React/1647833712980-c2c34d54-f30f-4fbb-9242-1851c0363d43-972702.png)

### 传统写法：

* <code><Switch></code> 替换成了 <code><Routes></code>
* <code>Route</code> 中的 <code>component</code> 替换成了 <code>element</code>

```tsx
import React from 'react';

import Login from '@/pages/login';
import Layout from '@/pages/Layout';
import Home from '@/pages/home';
import About from '@/pages/about';
import Archives from '@/pages/archives';
import ArchivesChild1 from '@/pages/archives/child1';
import ArchivesChild2 from '@/pages/archives/child1';

import {ConfigProvider} from 'antd';
import zhCN from 'antd/lib/locale/zh_CN';
import {HashRouter, Route, Routes} from 'react-router-dom';

const Index = () => {
  return (
    <>
      <ConfigProvider locale={zhCN}>
        <HashRouter>
          <Routes>
            <Route path='/login' element={<Login/>}/>
            <Route path='/' element={<Layout/>}>
              <Route path='home' element={<Home/>}/>
              <Route path='about' element={<About/>}/>
              <Route path='archives' element={<Archives/>}>
                <Route path='child1' element={<ArchivesChild1/>}/>
                <Route path='child2' element={<ArchivesChild2/>}/>
              </Route>
            </Route>
          </Routes>
        </HashRouter>
      </ConfigProvider>
    </>
  )
};

export default Index;
```

* 新增\*\* **<code>**<Outlet />**</code>** \*\*视图窗口组件：

```tsx
import React from 'react';
import {Outlet} from "react-router-dom";

const Index = () => {
  return (
    <div>
      <Outlet/>
    </div>
  );
};

export default Index;
```

```tsx
import React from 'react';
import {Outlet} from "react-router-dom";

const Index = () => {
  return (
    <div>
      archives page
      <Outlet/>
    </div>
  );
};

export default Index;
```

### 将 `routes` 抽离写法：

```tsx
import {RouteProps} from 'react-router-dom';

import Login from '@/pages/login';
import Layout from '@/pages/Layout';
import Home from '@/pages/home';
import About from '@/pages/about';
import Archives from '@/pages/archives';
import ArchivesChild1 from '@/pages/archives/child1';
import ArchivesChild2 from '@/pages/archives/child1';

interface IRouteProps extends RouteProps {
  children?: IRouteProps[]
}

const Routes: IRouteProps[] = [
  {path: '/login', element: <Login/>},
  {
    path: '/', element: <Layout/>,
    children: [
      {path: 'home', element: <Home/>},
      {path: 'about', element: <About/>},
      {
        path: 'archives', element: <Archives/>,
        children: [
          {path: 'child1', element: <ArchivesChild1/>},
          {path: 'child2', element: <ArchivesChild2/>},
        ]
      },
    ]
  },
]

export default Routes;
```

```tsx
import React from 'react';
import routes from './routes';
import {ConfigProvider} from 'antd';
import zhCN from 'antd/lib/locale/zh_CN';
import {HashRouter, Route, Routes, RouteProps} from 'react-router-dom';

interface IRouteProps extends RouteProps {
  children?: IRouteProps[]
}

const routerRender = (routes: IRouteProps[]) => {
  return routes?.map((item: IRouteProps, index: number) => {
    return (
      <Route key={index} path={item.path} element={item.element}>
        {item.children ? routerRender(item.children) : null}
      </Route>
    )
  })
}

const Index = () => {
  return (
    <>
      <ConfigProvider locale={zhCN}>
        <HashRouter>
          <Routes>{routerRender(routes)}</Routes>
        </HashRouter>
      </ConfigProvider>
    </>
  )
};

export default Index;
```


