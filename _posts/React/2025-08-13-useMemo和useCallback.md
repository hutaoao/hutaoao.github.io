---
title: useMemo和useCallback
date: 2025-08-13
description: useMemo和useCallback
tags: [React]
categories: [React]
---
# useMemo和useCallback

`useMemo` 和 `useCallback` 都是 `React` 进行性能优化的手段，

<font style="color:rgb(77, 77, 77);">这两个 hooks 的作用就是进行</font>**<font style="color:rgb(77, 77, 77);">数据缓存，</font>\*\*\*\*<font style="color:#DF2A3F;">避免页面重新渲染时不必要的参数重复更新</font>**<font style="color:rgb(77, 77, 77);">。</font>

<font style="color:rgb(77, 77, 77);">它们的适用场景就是</font>**<font style="color:rgb(13, 0, 22);">对于只需要单次进行复杂计算的内容进行数据存储，从而</font>\*\*\*\*<font style="color:rgb(77, 77, 77);">减少页面性能开销。</font>**

**<font style="color:rgb(77, 77, 77);"></font>**

**<font style="color:rgb(77, 77, 77);">useMemo</font>**

```tsx
import {useEffect, useMemo, useState} from "react";

export const UseMemoExample = () => {
  const [count, setCount] = useState(0);
  // 这里useMemo的依赖参数是count，因此count每次变化的时候，useMemo就会跟着变，那么如果第二个参数是一个固定值，或者其他变量的话，useMemo就不会在每次重新render的时候执行了，同理与useCallback
  const initCount = useMemo(() => {
    console.log('执行useMemo');
    return count + 1;
  }, [count]);


  useEffect(() => {
    console.log('执行useEffect');
  },[count]);

  return (
    <div>
      <p>count {count}</p>
      <p>initCount {initCount}</p>
      <button onClick={() => setCount(count + 1)}>click</button>
    </div>
  )
}
```


