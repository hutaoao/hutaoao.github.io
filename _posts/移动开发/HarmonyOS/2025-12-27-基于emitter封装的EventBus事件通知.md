---
title: 基于emitter封装的EventBus事件通知
date: 2025-12-27
description: 基于 emitter 封装的 EventBus 事件通知
tags: [harmonyos]
categories: [移动开发, HarmonyOS]
---
# 基于 emitter 封装的 EventBus 事件通知

官网模块入口：

[文档中心](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/js-apis-emitter-V5#emitteremit)

### 工具类

```typescript
import { emitter } from '@kit.BasicServicesKit';

/// 事件通知工具类
export abstract class EventBus {
  static send(
    eventID: string,
    eventData?: Object | Record<string, Object> | null,
  ) {
    let data: string | undefined
    if (eventData !== null && eventData !== undefined) {
      if (typeof eventData === 'string') {
        data = eventData
      } else {
        data = JSON.stringify(eventData)
      }
    }

    emitter.emit(
      eventID,
      {
        priority: emitter.EventPriority.HIGH
      },
      {
        data: {
          'data': data
        }
      },
    );
  }

  static listen<T>(
    eventID: string,
    callback: (data?: T) => void,
  ) {
    emitter.on(
      eventID,
      (eventData: emitter.EventData) => {
        let data: string | undefined = eventData.data!['data']
        if (data == undefined) {
          callback(undefined)
        } else {
          if (data.startsWith("{") && data.endsWith("}")) {
            callback(JSON.parse(data) as T)
          } else {
            callback(data as T)
          }
        }
      },
    );
  }

  static cancel(eventID: string) {
    emitter.off(eventID);
  }
}
```

### 使用

#### 发送事件

<code>**EventBus.send('name')**</code>

#### 监听事件

```typescript
EventBus.listen<ImgInterface>('name', (e)=> {
  console.log('click mine tab')
})
```

#### 关闭事件

<code>**EventBus.cancel('name')**</code>

�


