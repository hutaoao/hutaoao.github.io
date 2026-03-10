---
title: 问题-Unknownargumenttype-__attribute__-inmethod-RCTAppStategetCurrentAppState_error_
date: 2025-12-15
description: 问题：Unknown argument type ‘attribute‘ in method -RCTAppState getCurrentAppState:error:.
tags: [ReactNative]
categories: [移动开发, ReactNative]
---
# 问题：Unknown argument type ‘__attribute__‘ in method -[RCTAppState getCurrentAppState:error:].

遇到的问题：

Xcode版本升级后，在Xcode上点击build时遇到以下问题：

```bash
Unknown argument type '__attribute__' in method -[RCTAppState getCurrentAppState:error:]. Extend RCTConvert to support this type.
```



如图：

![1626069499588-84d85c09-67f0-429f-b2d0-9ad43a752a7a.png](/assets/img/posts/移动开发/ReactNative/1626069499588-84d85c09-67f0-429f-b2d0-9ad43a752a7a-864271.png)

**解决的方法：**

1.在Xcode中打开：Xcode打开RCTModuleMethod.mm文件：路径Libraries->React.xcodeproj->React->Base ->RCTModuleMethod.mm

![1626069518608-e307acb1-76d4-49a5-a52a-5b3c0545fdc6.png](/assets/img/posts/移动开发/ReactNative/1626069518608-e307acb1-76d4-49a5-a52a-5b3c0545fdc6-128022.png)

2.更改代码：

![1626069532103-62886f3b-b6f2-4274-9b8c-697430b5f1e2.png](/assets/img/posts/移动开发/ReactNative/1626069532103-62886f3b-b6f2-4274-9b8c-697430b5f1e2-924248.png)

```plain
static BOOL RCTParseUnused(const char **input)
{
  return RCTReadString(input, "attribute((unused))") ||
         RCTReadString(input, "__attribute__((__unused__))") ||
         RCTReadString(input, "__unused");
}
```



3.clean Xcode ：click product->clean or shift+cmd+k



4.重新build，成功！

[  
](https://blog.csdn.net/koufulong/article/details/107279920)



