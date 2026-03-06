---
title: JavaScript中breakcontinue和return的用法
date: 2025-10-01
description: JavaScript中break continue和return的用法
tags: [Javascript, javascript]
categories: [Javascript]
---
# JavaScript中break continue和return的用法

### break


跳出代码块（循环），使用于循环和 switch 等。



```javascript
for (let i = 1; i <= 10; i++) {
    if (i === 6) {
        break;
    }
    console.log(i);//1 2 3 4 5
}
//当 i=6 的时候，直接退出fo循环，这个循环将不再被执行！
```



### continue


跳过循环中的一个迭代，进入下一个迭代，用于循环的代码块。



```javascript
for (let i = 1; i <= 10; i++) {
    if (i === 6) {
        continue;
    }
    console.log(i);//1 2 3 4 5 7 8 9 10
}
//当 i=6 的时候，直接跳出本次for循环，然后继续循环中的下一个迭代。。
```



### return


终止函数的执行并返回函数的值，应用范围只能出现在函数体内。



我们通常 **return false 来阻止提交表单或者继续执行下面的代码，通俗的来说就是阻止执行默认的行为。**



> 总之：return false 只在当前函数有效，不会影响其他外部函数的执行。
>



1、返回控制与函数结果



语法为：return 表达式;



语句结束函数执行，返回调用函数，而且把表达式的值作为函数的结果。



```javascript
function format(time) {
    if(time <10){
        return '0'+time;
    }else{
        return time;
    }
}
console.log(format(5));//05
```



2、返回控制



无函数结果，语法为：return;



在大多数情况下,为事件处理函数返回false，可以防止默认的事件行为；我们也常用return false来阻止提交表单或者继续执行下面的代码。



**Return False 就相当于终止符，Return True 就相当于执行符。 返回的false和true通常用在需要进行布尔类型判断时。**



比如你单击一个链接，除了触发你的onclick事件（如果你指定的话）以外还要触发一个默认事件就是执行页面的跳转。所以如果你想取消对象的默认动作就可以return false。



```javascript
<a href="https://www.baidu.com" onclick=" return fun()">点击</a>

<script type="text/javascript">
    function fun(){
        location.href="https://www.sina.com.cn";
        return false;
    }
</script>

//单击超链接后会跳转到新浪而不会跳转到百度，如果没有renturn false 则会跳转到百度
```



```javascript
function a() {
    if(true){
        return false;
    }
}
function text() {
    a();
    b();
    c();
}
//即使a函数返回return false 阻止提交了，但是不影响  b() 以及 c() 函数的执行。
//return false 对于 Test() 函数来说，只是相当于返回值。而不能阻止 Test() 函数执行。
```



**总结：**



+ return true；返回正常的处理结果；终止处理。
+ return false；返回错误的处理结果；终止处理；阻止提交表单；阻止执行默认的行为。
+ return；把控制权返回给页面。



> 更新: 2019-11-25 10:22:42  
> 原文: <https://www.yuque.com/hutaoao/blog/raytpg>