---
title: getCurrentInstance
date: 2025-08-22
description: getCurrentInstance
tags: [Vue, Vue3]
categories: [Vue, Vue3]
---
# getCurrentInstance

getCurrentInstance 支持**访问内部组件实例**。



!!!WARNING

+ getCurrentInstance 只暴露给高阶使用场景，典型的比如在库中。强烈反对在应用的代码中使用；
+ getCurrentInstance。请不要把它当作在组合式 API 中获取 this 的替代方案来使用。



```vue
import { getCurrentInstance } from 'vue'

const MyComponent = {
  setup() {
    // 获取当前组件的上下文，下面两种方式都能获取到组件的上下文。
    const { ctx }  = getCurrentInstance();  //  方式一，这种方式只能在开发环境下使用，生产环境下//的ctx将访问不到
    const { proxy }  = getCurrentInstance();  //  方式二，此方法在开发环境以及生产环境下都能放到组件上下文对象（推荐）
  }
}
```



**getCurrentInstance 只能在 setup 或生命周期钩子中调用。**

> 如需在 setup 或生命周期钩子外使用，请先在 setup 中调用 getCurrentInstance() 获取该实例然后再使用。



```vue
const MyComponent = {
  setup() {
    const internalInstance = getCurrentInstance() // 有效
 
    const id = useComponentId() // 有效
 
    const handleClick = () => {
      getCurrentInstance() // 无效
      useComponentId() // 无效
 
      internalInstance // 有效
    }
 
    onMounted(() => {
      getCurrentInstance() // 有效
    })
 
    return () =>
      h(
        'button',
        {
          onClick: handleClick
        },
        `uid: ${id}`
      )
  }
}
 
// 在组合式函数中调用也可以正常执行
function useComponentId() {
  return getCurrentInstance().uid
}
```





