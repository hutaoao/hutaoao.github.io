---
title: React中的Hooks
date: 2025-07-30
description: React 中的 Hooks
tags: [React, react]
categories: [Web前端, React]
---
# React 中的 Hooks

#### useState

1. useState（initialState）有一个初始化值作为参数，initialState 可以是任意值
2. 返回包含两个值数组，第一个是值，第二个是 set 方法

**需要注意的一点，setCount 后，直接获取 count,count 的值是不变的，这是因为 useState 是一个异步操作，它并不会立即更新状态。当你在函数组件中调用 useState 更新状态时，React 会将新的状态安排进下一次渲染，并在之后的某个时候才会执行更新，这导致在更新状态后立即访问该状态可能会得到之前的值，而不是最新的值。**

```jsx
//这是一个useState简单使用实例
function Plus() {
  const [count, setCount] = useState(1);
  function counter(){
    setCount(count+1)
  }
  return (
    <div className='plus'>
      <div>{count}</div>
      <div className='plus-click' onClick={counter}>+</div>

    </div>
  );
}
```

#### useEffect（执行副作用）

**副作用是指一段和当前执行结果无关的代码**，比如说要修改函数外部的某个变量，要发起一个请求有两个参数。useEffect 有两个参数第一个参数是 callback，第二个是个依赖数组，这个数组可以为空

```jsx
// 在这里count的值发生变化时，clikValue的值及跟着变化，在这个callback里也可以发起网络请求
function CountPlus() {
  const [count, setCount] = useState(0);
  const [clikValue, setClickValue] = useState("");

  useEffect(() =>{
    setClickValue("我点击了"+count+"次")

  },[count])
  function counter(){
    setCount(count+1)
  }
  return (
    <div className='plus'>
      <div>{count}</div>
      <div className='plus-click' onClick={counter}>+</div>
      <div>{clikValue}</div>

    </div>
  );
}
export default CountPlus;
```

#### useContext

可以**用来在组件树中传递数据**

```jsx
//User：是消费提供的值的地方
import React, { useContext  } from 'react';
import UserContext from './UserContext';
import '.././index.css';

function User() {
  const user = useContext(UserContext)

  return (
    <div className='plus'>
      <p>{user.name}</p>
      <p>{user.sex}</p>
    </div>
  );
}
export default User;
```

```jsx
// UserContext：创建上下文

import { createContext } from 'react';

const UserContext = createContext();

export default UserContext;
```

```jsx
//App：是提供上下文值的地方
import User from "./componts/User";
import UserContext from "./componts/UserContext";
import './App.css';
function App() {
  return (
    <div className="App">
{% raw %}
      <UserContext.Provider value={{ name: '张三', sex: '男' }}>
{% endraw %}
        <User></User>
      </UserContext.Provider>
    </div>
  );
}

export default App;
```

#### useReducer

用于在函数组件中处理复杂的状态逻辑。它通常用于管理具有复杂状态和行为的组件，尤其是涉及到多个状态转换的情况。useReducer 接受两个参数：一个是包含状态转换逻辑的函数（reducer），另一个是初始状态。它返回一个包含当前状态和 dispatch 函数的数组。

```jsx
import CountDis from "./componts/CountDis";
import './App.css';
import React, { useReducer } from 'react'
const reducer = (state, action) => {
  switch (action.type) {
    case 'count':
      return { ...state, count: state.count + 1 }
    default:
      return state
  }
}
function App() {
  const [useState, dispatch] = useReducer(reducer, { count: 0 })
  return (
    <div className="App">
      <p>点击了 {useState.count}次</p>
      <CountDis dispatch={dispatch}></CountDis>
    </div>
  );
}

export default App;
```

```jsx
import React from 'react';
import '.././index.css';

function CountDis({dispatch}) {

  function counter(){
    dispatch({ type: 'count' })
  }
  return (
    <div className='plus'>
      <div className='plus-click' onClick={counter}>+</div>
    </div>
  );
}
export default CountDis;
```

#### useRef

useRef 主要用于在函数组件中保存和访问可变的 ref 对象。useRef 返回一个包含 current 属性的对象，该属性被初始化为传入的参数（任意数据类型） 主要作用

* 访问 Dom 元素
* 保存变量、

```jsx
function CountRef() {
  const inputRef = useRef(null);
  // 使用 ref 访问 DOM 元素
  useEffect(() => {
    if (inputRef.current) {
      inputRef.current.focus();
    }
  }, []);
  function counter() {
    alert(inputRef.current.value)
  }
  return (
    <div className='plus'>
      <p>{inputRef.current}</p>
      <input ref={inputRef}></input>
      <div className='plus-click' onClick={counter}>+</div>
    </div>
  );
}
export default CountRef;
```

#### useCallback

useCallback 的主要目的是性能优化 用于缓存回调函数，以便在依赖项变化时避免不必要的函数重新创建。在 React 中，每当函数组件重新渲染时，函数组件的所有局部变量都会重新初始化。如果将一个新的函数作为 prop 传递给子组件，即使这个函数具有相同的逻辑，由于它是一个新的引用，子组件可能会被重新渲染，从而可能引起性能问题。

```jsx
function CountUseCallback() {
  const [count, setCount] = useState(1);
  const handleClick = useCallback(() => {
    // 处理点击事件逻辑
    setCount(count+1)
  }, []);
  return (
    <div className='plus'>
      <div>{count}</div>
      <div className='plus-click' onClick={handleClick}>+</div>

    </div>
  );
}
export default CountUseCallback;
```

#### useMemo

用于在渲染过程中缓存和重用计算昂贵的值。它类似于 useCallback，但是主要用于缓存计算结果，而不是缓存回调函数。

```jsx
function CountMemo({size}) {
  let result = useMemo(() => {
    return comuteData()
  }, [size])

  function comuteData(){
    if(size<0){
      return 0
    }
    let sum = 0
    for(let i =1;i<=size;i++){
      sum+=i
    }
    return sum
  }

  return (
    <div className='plus'>
      <p>{result}</p>


    </div>
  );
}
export default CountMemo;
```

### 总结

上述的钩子里 useState，useEffect，useReducer，useRef 本人用的较为多些，useContext ，useMemo 和 useCallback 用的少些。除了上述钩子之外，还有 useImperativeHandle、useLayoutEffect、自定义钩子等其他用途的钩子。通过深入了解和熟练使用这些钩子，我们可以更有效地构建强大而灵活的 React 应用程序。在使用钩子时，选择适当的钩子来解决您的问题，并根据需要组合使用它们，以获得最佳性能和可维护性。


