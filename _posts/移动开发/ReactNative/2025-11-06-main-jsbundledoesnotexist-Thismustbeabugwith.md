---
title: main-jsbundledoesnotexist-Thismustbeabugwith
date: 2025-11-06
description: main.jsbundle does not exist. This must be a bug with
tags: [移动开发, ReactNative, 记录奇奇怪怪问题]
categories: [移动开发, ReactNative]
---
# main.jsbundle does not exist. This must be a bug with

![1669967132118-343d9610-9d96-4b24-8f7f-725b4280b9e8.png](/assets/img/posts/移动开发/ReactNative/1669967132118-343d9610-9d96-4b24-8f7f-725b4280b9e8-542178.png)

**！！！最新处理方案：**

由于M1 xcode 架构变化导致，需要软链一下 <code>watchman</code>

> cd /opt/homebrew/bin
> sudo ln -s  /opt/homebrew/bin/watchman /usr/local/bin/watchman

***

1）在 `package.json` `script` 中 添加

```plain
"buildIos": "react-native bundle --entry-file index.js --platform ios --dev false --bundle-output release_ios/main.jsbundle --assets-dest release_ios/"
```

�

2）然后执行 `npm run buildIos`

会在`release_ios`目录生成一个 `main.jsbundle` 文件

3）然后如下图 引入 该文件 `main.jsbundle`

![1669966142156-20a6d6f0-ab35-4378-865a-41dce0a83f61.png](/assets/img/posts/移动开发/ReactNative/1669966142156-20a6d6f0-ab35-4378-865a-41dce0a83f61-705556.png)

或：

![1669968094626-fd2cf7cd-6718-4c26-a3fd-141b17eb40df.png](/assets/img/posts/移动开发/ReactNative/1669968094626-fd2cf7cd-6718-4c26-a3fd-141b17eb40df-736490.png)


