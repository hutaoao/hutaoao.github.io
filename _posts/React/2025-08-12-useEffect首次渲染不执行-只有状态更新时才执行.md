---
title: useEffect首次渲染不执行-只有状态更新时才执行
date: 2025-08-12
description: useEffect 首次渲染不执行，只有状态更新时才执行
tags: [React]
categories: [React]
---
# useEffect 首次渲染不执行，只有状态更新时才执行

使用useRef：

```jsx
const isMounted = useRef(false);

useEffect(() => {
  if(!isMounted.value) {
    isMounted.value = true
  } else {
    console.log('只在deps更新时执行')
  }
},deps)
```



下面几种首次渲染都会执行：

```jsx
useEffect(() => {
  console.log('空数组');
}, []);

useEffect(() => {
  console.log('每次执行');
})

useEffect(() => {
  console.log('依赖数组');
}, [age])
```



