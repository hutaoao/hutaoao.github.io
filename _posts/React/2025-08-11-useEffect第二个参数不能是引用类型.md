---
title: useEffect第二个参数不能是引用类型
date: 2025-08-11
description: useEffect第二个参数不能是引用类型
tags: [React]
categories: [React]
---
# useEffect第二个参数不能是引用类型

```javascript
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState({});

  useEffect(() => {
   setCount({test:"count是一个对象，使得页面死循环"})
  },[count]);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

```

**结果：页面死循环**

**<font style="color:#F5222D;"></font>**

**解释：**

useEffect接受两个参数：

+ 第一个参数是函数（这里叫effect函数），它的作用是，在页面渲染后执行这个函数。因此你可以把ajax请求等放在这里执行；
+ 第二个参数是一个数组，这里注意：

| 参数情况 | 效果 | 注意 |
| :---: | :---: | :---: |
| 不传 | 每次渲染后都执行清理或者执行effect | 这可能会导致性能问题，比如两次渲染的数据完全一样 |
| 传空数组 | 只运行一次的 effect（仅在组件挂载和卸载时执行） | 这就告诉 React 你的 effect 不依赖于 props 或 state 中的任何值，所以它永远都不需要重复执行 |
| 传[count] | React 将对前一次渲染的count和后一次渲染的count进行比较。若相等React 会跳过这个 effect， | 实现了性能的优化 |


上面例子中之所以造成页面的死循环，是因为在JavaScript中，{} === {}结果是false，{a:1} === {a:1}同样，由此造成了react以为两个值不同，就一直的渲染最终页面死循环。



**<font style="color:#F5222D;">结论：第二个参数一般情况下不要使用引用类型！</font>**



