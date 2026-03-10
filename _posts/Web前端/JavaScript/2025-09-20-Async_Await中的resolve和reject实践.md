---
title: Async_Await中的resolve和reject实践
date: 2025-09-20
description: Async/Await中的resolve和reject实践
tags: [javascript]
categories: [Web前端, JavaScript]
---
# Async/Await中的resolve和reject实践

```javascript
<script>
  const func1 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('1');
        resolve('11');
      }, 1000)
    })
  }

  const func2 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('2');
        resolve('22');
      }, 1000)
    })
  }

  (async function () {
    console.log('0');
    const one = await func1();
    console.log(one);
    const two = await func2();
    console.log(two);
  }())

// 0
两秒后
// 1
// 11
四秒后
// 2
// 22
</script>
```

***

如果把 `func1` 的 `resolve()` 注释掉：

```javascript
<script>
  const func1 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('1');
        // resolve('11');
      }, 1000)
    })
  }

  const func2 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('2');
        resolve('22');
      }, 1000)
    })
  }

  (async function () {
    console.log('0');
    const one = await func1();
    console.log(one);
    const two = await func2();
    console.log(two);
  }())

// 0
两秒后
// 1
</script>
```

此时由于 `func1` 没有 `resolve()` 和 `reject()` ，会阻塞 `func2()` 的执行。

***

把 func1 的 `resolve()` 改为 `reject()`：

```javascript
<script>
  const func1 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('1');
        reject('11');
      }, 1000)
    })
  }

  const func2 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('2');
        resolve('22');
      }, 1000)
    })
  }

  (async function () {
    console.log('0');
    const one = await func1();
    console.log(one);
    const two = await func2();
    console.log(two);
  }())

// 0
// 两秒后
// 1
// 报错：Uncaught (in promise) 11
</script>
```

***

再用 try-catch 检测 `func1()` 错误：

```javascript
<script>
  const func1 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('1');
        reject('err - 11');
      }, 1000)
    })
  }

  const func2 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('2');
        resolve('22');
      }, 1000)
    })
  }

  (async function () {
    console.log('0');
    try {
      const one = await func1();
      console.log(one);
    } catch (err) {
      console.log(err)
    }
    const two = await func2();
    console.log(two);
  }())

// 0
两秒后
// 1
// err - 11
四秒后
// 2
// 22
</script>
```

监测到 `func1()` 的错误，发现没有阻塞 `func2()` 的执行。

***

把 await func1() 和 await func2() 都放入 try 里面：

```javascript
<script>
  const func1 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('1');
        reject('err - 11');
      }, 1000)
    })
  }

  const func2 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('2');
        resolve('22');
      }, 1000)
    })
  }

  (async function () {
    console.log('0');
    try {
      const one = await func1();
      console.log(one);
      const two = await func2();
   		console.log(two);
    } catch (err) {
      console.log(err)
    }
  }())

// 0
两秒后
// 1
// err - 11
</script>
```

同样阻塞。

***

此时如果 func1() 没有 `resolve()` 和 `reject()`：

```javascript
<script>
  const func1 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('1');
        // reject('err - 11');
      }, 1000)
    })
  }

  const func2 = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('2');
        resolve('22');
      }, 1000)
    })
  }

  (async function () {
    console.log('0');
    try {
      const one = await func1();
      console.log(one);
    } catch (err) {
      console.log(err)
    }
    const two = await func2();
    console.log(two);
  }())

// 0
// 两秒后
// 1
</script>
```

没有报错，会阻塞 `func2()`。


