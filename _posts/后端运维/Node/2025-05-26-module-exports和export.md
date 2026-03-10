---
title: module-exports和export
date: 2025-05-26
description: module.exports 和 export
tags: [后端运维, node, npm]
categories: [后端运维, Node]
---
# module.exports 和 export

**CommonJS模块规范 **和 **ES6模块规范 **完全是两种不同的概念

## CommonJS模块规范

**Node**应用由模块组成，采用CommonJS模块规范。

CommonJS规范规定，每个模块内部，module变量代表当前模块。这个变量是一个对象，它的exports属性（即module.exports）是对外的接口。加载某个模块，其实是加载该模块的module.exports属性。

`module` 变量代表当前模块。这个变量是一个对象，module对象会创建一个叫exports的属性，这个属性的默认值是一个空的对象：

```javascript
module.exports = {};
```

```javascript
var x = 5;
var addX = function (value) {
  return value + x;
};
module.exports.x = x;
module.exports.addX = addX;

// 上边这段代码就相当于d一个对象
{
	x = 5,
  addX = function (value) {
    return value + x;
  }
}
```

上面代码通过module.exports输出变量x和函数addX。

require方法用于加载模块。

```javascript
var example = require('./example.js');

console.log(example.x); // 5
console.log(example.addX(1)); // 6
```

### exports 与 module.exports的关系

Node为每个模块提供一个exports变量，指向module.exports。可以通俗的理解为：

```javascript
var exports = module.exports;

// 两个是相等的关系，但又不是绝对相当的关系
// 例如：
// 1.module.exports可以直接导出一个匿名函数或者一个值

module.exports = function() {
  var a = "Hello World";
  return a;
}

// 但是exports是不可以的，因为这样等于切断了exports与module.exports的联系。
exports = function(){        //这样写法是错误的
  var a = "Hello World";     //这样写法是错误的
  return a;                  //这样写法是错误的
}
```



## ES6模块规范

不同于CommonJS，ES6使用 export 和 import 来导出、导入模块。



### export和export default的区别

* export default在一个模块中只能有一个，当然也可以没有。export在一个模块中可以有多个。
* export default的对象、变量、函数、类，可以没有名字。export的必须有名字。
* export default对应的import和export有所区别

### export命令

```javascript
// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var say = function(){
 console.log("我可以干很多事");
}

export {firstName, lastName, say};

// 使用
import {firstName, lastName, say} from 'profile.js';

//也可以直接一个一个的 export 但是必须得有名字
export const a = 1;
export function data(){
　return data;
}
```

需要特别注意的是，export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。

```javascript
// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};

// 使用
import {m} from 'profile.js';
```

### export default 命令

```javascript
// example.js
export default function () {
  console.log('foo');
}

//使用
import example from 'example.js';

// 注意
//这里export default不能这样导出
export default const a=12;
//应该这样导出
const a=12;
export default a;
```


