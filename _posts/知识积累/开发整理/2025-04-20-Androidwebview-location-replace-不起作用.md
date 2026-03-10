---
title: Androidwebview-location-replace-不起作用
date: 2025-04-20
description: Android webview “location.replace” 不起作用
tags: [开发整理, android, web]
categories: [知识积累, 开发整理]
---
# Android webview “location.replace” 不起作用

```javascript
function locationReplace(url){
  if(history.replaceState){
    history.replaceState(null, document.title, url);
    history.go(0);
  }else{
     location.replace(url);
  }
}
```



