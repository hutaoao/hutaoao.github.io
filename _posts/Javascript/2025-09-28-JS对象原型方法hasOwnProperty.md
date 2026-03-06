---
title: JS对象原型方法hasOwnProperty
date: 2025-09-28
description: JS对象原型方法 hasOwnProperty()
tags: [Javascript]
categories: [Javascript]
---
# JS对象原型方法 hasOwnProperty()

**<font style="color:rgb(0, 0, 0);">Object的</font>****<font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">hasOwnProperty()</font>****<font style="color:rgb(0, 0, 0);">方法返回一个</font>****<font style="color:#F5222D;">布尔值</font>****<font style="color:rgb(0, 0, 0);">，判断对象是否包含特定的自身（</font>****<font style="color:#F5222D;">非继承</font>****<font style="color:rgb(0, 0, 0);">）属性。</font>**

## <font style="color:rgb(0, 0, 0);">判断自身属性是否存在</font>
```javascript
var o = new Object();
o.prop = 'exists';

function changeO() {
  o.newprop = o.prop;
  delete o.prop;
}

o.hasOwnProperty('prop');  // true
changeO();
o.hasOwnProperty('prop');  // false
```

## <font style="color:rgb(0, 0, 0);">判断自身属性与继承属性</font>
```javascript
function foo() {
  this.name = 'foo'
  this.sayHi = function () {
    console.log('Say Hi')
  }
}

foo.prototype.sayGoodBy = function () {
  console.log('Say Good By')
}

let myPro = new foo()

console.log(myPro.name) // foo
console.log(myPro.hasOwnProperty('name')) // true
console.log(myPro.hasOwnProperty('toString')) // false
console.log(myPro.hasOwnProperty('hasOwnProperty')) // fasle
console.log(myPro.hasOwnProperty('sayHi')) // true
console.log(myPro.hasOwnProperty('sayGoodBy')) // false
console.log('sayGoodBy' in myPro) // true
```

## <font style="color:rgb(0, 0, 0);">遍历一个对象的所有自身属性</font>
<font style="color:rgb(0, 0, 0);">在看开源项目的过程中，经常会看到类似如下的源码。</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">for...in</font><font style="color:rgb(0, 0, 0);">循环对象的所有枚举属性，然后再使用</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">hasOwnProperty()</font><font style="color:rgb(0, 0, 0);">方法来忽略继承属性。</font>

```javascript
var buz = {
    fog: 'stack'
};

for (var name in buz) {
    if (buz.hasOwnProperty(name)) {
        alert("this is fog (" + name + ") for sure. Value: " + buz[name]);
    }
    else {
        alert(name); // toString or something else
    }
}
```

## <font style="color:rgb(0, 0, 0);">注意</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">hasOwnProperty</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">作为属性名</font>
**<font style="color:rgb(0, 0, 0);">JavaScript 并没有保护 </font>****<font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">hasOwnProperty</font>****<font style="color:rgb(0, 0, 0);"> 属性名</font>**<font style="color:rgb(0, 0, 0);">，因此，可能存在于一个包含此属性名的对象，有必要使用一个可扩展的</font><font style="color:rgb(232, 62, 140);background-color:rgb(246, 246, 246);">hasOwnProperty</font><font style="color:rgb(0, 0, 0);">方法来获取正确的结果：</font>

```javascript
var foo = {
    hasOwnProperty: function() {
        return false;
    },
    bar: 'Here be dragons'
};

foo.hasOwnProperty('bar'); // 始终返回 false

// 如果担心这种情况，可以直接使用原型链上真正的 hasOwnProperty 方法
// 使用另一个对象的`hasOwnProperty` 并且call
({}).hasOwnProperty.call(foo, 'bar'); // true

// 也可以使用 Object 原型上的 hasOwnProperty 属性
Object.prototype.hasOwnProperty.call(foo, 'bar'); // true
```



> 更新: 2021-10-20 15:22:16  
> 原文: <https://www.yuque.com/hutaoao/blog/fzgqsa>