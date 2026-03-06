---
title: Javascript禁止前端调试页面
date: 2025-04-24
description: Javascript禁止前端调试页面
tags: [开发整理, javascript]
categories: [开发整理]
---
# Javascript禁止前端调试页面

![1712049548053-2739faae-bdc3-4158-a65b-244d68dfa8da.png](/assets/img/posts/开发整理/1712049548053-2739faae-bdc3-4158-a65b-244d68dfa8da-172688.png)

## 一、为什么要禁止？
由于前端页面会调用很多接口，有些接口会被别人爬虫分析，破解后获取数据，为了杜绝这种情况，最简单的方法就是禁止人家调试自己的前端代码。**特别是作为个人站长，不仅要掌握爬别人的网站，更要掌握如何防止被别人爬**。

## 二、如何实现？
别人爬我们的网站，一般都会先分析我们的网站代码，这时是需要使用F12查看我们的前端代码结构的，所以我们的思路就是从禁止开发者工具到禁止debugger一步步入手。

### 2.1、禁止右键菜单
这个比较简单，直接上代码：



```javascript
document.oncontextmenu = function() {
  console.log("小站不容易，请放过");
  alert("小站不容易，请放过");
  return false;
};
```

### 2.2、禁止F12
废话不多说，上代码：



### 2.3、禁止开发者工具
这里有2种实现思路：开发者工具打开后改变页面dom及url、无限debugger

2.3.1、改变dom及url



2.3.2、无限debugger

+ 基础禁止调试

前端页面防止调试的方法主要是通过不断 debugger 来疯狂输出断点，因为 debugger 在控制台被打开的时候就会执行由于程序被 debugger 阻止，所以无法进行断点调试，所以网页的请求也是看不到的。

![1712049547890-d04416d7-8d5d-4d7d-91bb-805b75368411.png](/assets/img/posts/开发整理/1712049547890-d04416d7-8d5d-4d7d-91bb-805b75368411-224720.png)

+ 基础禁止调试的对策

如果仅仅是加上面那么简单的代码，对于一些技术人员而言作用不大,可以通过控制台中的 Deactivate breakpoints 按钮或者使用快捷键 Ctrl + F8 关闭无限 debugger，这种方式虽然能去掉碍眼的 debugger，但是无法通过左侧的行号添加 breakpoint取消禁止对策。

![1712049548325-9fd730bc-a992-49c8-ad71-886bf0e5f86f.png](/assets/img/posts/开发整理/1712049548325-9fd730bc-a992-49c8-ad71-886bf0e5f86f-098025.png)

+ 禁止断点的对策

如果将 setInterval 中的代码写在一行，就能禁止用户断点，即使添加 logpoint 为 false 也无用，当然即使有些人想到用左下角的格式化代码，将其变成多行也是没用的。



![1712049548315-f96a9f9b-2cbf-471c-b097-4bc6c9fe4855.png](/assets/img/posts/开发整理/1712049548315-f96a9f9b-2cbf-471c-b097-4bc6c9fe4855-701477.png)

+ 忽略执行的代码

通过添加 add script ignore list 需要忽略执行代码行或文件，也可以达到禁止无限 debugger，忽略执行的代码。

![1712049548316-a9a8e285-92f6-4117-818b-99aacb0a9f76.png](/assets/img/posts/开发整理/1712049548316-a9a8e285-92f6-4117-818b-99aacb0a9f76-281874.png)

+ 忽略执行代码的对策

那如何针对上面操作的恶意用户呢? 可以通过将 debugger 改写成 Function(“debugger”)(); 的形式来应对！！Function 构造器生成的 debugger 会在每一次执行时开启一个临时 js 文件，当然使用的时候，为了更加的安全，最好使用加密后的脚本。



+ **终极增强防调试代码**

为了让自己写出来的代码更加的晦涩难懂，需要对上面的代码再优化一下。将 Function(‘debugger’).call() 改成 **(function(){return false;})‘constructor’‘call’;**并且添加条件，当窗口外部宽高和内部宽高的差值大于一定的值 ，我把 body 里的内容换成指定内容，当然使用的时候，为了更加的安全，最好加密后再使用。



![1712049549030-aa32b166-206d-4580-abf4-9f11c01202e1.png](/assets/img/posts/开发整理/1712049549030-aa32b166-206d-4580-abf4-9f11c01202e1-122199.png)

## 三、总结
其实上面的各种方式并不能100%防止，毕竟javascript是客户端语言，就说别人把你的网站页面全部下载下来，然后把里面的js拦截代码一删，在本地一样嘎嘎跑，想咋调试就咋调试，所以说到底还是得从服务端想办法。

```javascript
document.onkeydown = document.onkeyup = document.onkeypress = function(event) {
  let e = event || window.event || arguments.callee.caller.arguments[0];
  if (e && e.keyCode == 123) {
    e.returnValue = false;
    return false;
  }
};
```

```javascript
let userAgent = navigator.userAgent;
if (userAgent.indexOf("Firefox") > -1) {
  // Firefox浏览器特定操作
  let checkStatus;
  let devtools = /./;
  devtools.toString = function() {
    checkStatus = "on";
  };
  setInterval(function() {
    checkStatus = "off";
    console.log(devtools);
    console.log(checkStatus);
    console.clear();
    if (checkStatus === "on") {
      try {
        window.open("about:blank", "_self");
      } catch (err) {
        let a = document.createElement("button");
        a.onclick = function() {
          window.open("about:blank", "_self");
        };
        a.click();
      }
    }
  }, 200);
} else {
  // 非Firefox浏览器特定操作
  let ConsoleManager = {
    onOpen: function() {
      alert("Console is opened");
    },
    onClose: function() {
      alert("Console is closed");
    },
    init: function() {
      let self = this;
      let x = document.createElement("div");
      let isOpening = false,
        isOpened = false;
      Object.defineProperty(x, "id", {
        get: function() {
          if (!isOpening) {
            self.onOpen();
            isOpening = true;
          }
          isOpened = true;
          return true;
        }
      });
      setInterval(function() {
        isOpened = false;
        console.info(x);
        console.clear();
        if (!isOpened && isOpening) {
          self.onClose();
          isOpening = false;
        }
      }, 200);
    }
  };
  ConsoleManager.onOpen = function() {
    // 打开控制台，跳转
    try {
      window.open("about:blank", "_self");
    } catch (err) {
      let a = document.createElement("button");
      a.onclick = function() {
        window.open("about:blank", "_self");
      };
      a.click();
    }
  };
  ConsoleManager.onClose = function() {
    alert("Console is closed!");
  };
  ConsoleManager.init();
}
```

```javascript
/**
* 基础禁止调试代码
*/
(() => {
  function ban() {
    setInterval(() => {
      debugger;
    }, 50);
  }
  try {
    ban();
  } catch (err) { }
})();
```

```javascript
(() => {
  function ban() {
    setInterval(() => { debugger; }, 50);
  }
  try {
    ban();
  } catch (err) { }
})();
```

```javascript
// 加密前
(() => {
  function ban() {
    setInterval(() => {
      Function('debugger')();
    }, 50);
  }
  try {
    ban();
  } catch (err) { }
})();

// 加密后
eval(function(c,g,a,b,d,e){d=String;if(!"".replace(/^/,String)){for(;a--;)e[a]=b[a]||a;b=[function(f){return e[f]}];d=function(){return"\w+"};a=1}for(;a--;)b[a]&&(c=c.replace(new RegExp("\b"+d(a)+"\b","g"),b[a]));return c}('(()=>{1 0(){2(()=>{3("4")()},5)}6{0()}7(8){}})();',9,9,"block function setInterval Function debugger 50 try catch err".split(" "),0,{}));
```

```javascript
(() => {
  function block() {
    if (window.outerHeight - window.innerHeight > 200 || window.outerWidth - window.innerWidth > 200) {
      document.body.innerHTML = "检测到非法调试,请关闭后刷新重试!";
    }
    setInterval(() => {
      (function () {
        return false;
      }
       ['constructor']('debugger')
       ['call']());
    }, 50);
  }
  try {
    block();
  } catch (err) { }
})();
```

  




