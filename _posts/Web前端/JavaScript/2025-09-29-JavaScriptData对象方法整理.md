---
title: JavaScriptData对象方法整理
date: 2025-09-29
description: JavaScript Data对象方法整理
tags: [javascript]
categories: [Web前端, JavaScript]
---
# JavaScript Data对象方法整理



> 我整理这些知识点是在2019年6月6号，为以下数据作为参考！





## 获取时间


### getFullYear()


从 Date 对象返回一个表示年份的 4 位数字。



```javascript
let d = new Date();
let n = d.getFullYear();//返回年份
console.log(n);//2019
```



### getMonth()


从 Date 对象返回表示月份的数字。返回值是 0（一月） 到 11（十二月） 之间的一个整数。



```javascript
let d = new Date();
let n = d.getMonth();//返回月份
console.log(n);//5
```



### getDate()


从 Date 对象返回一个月中 (1 ~ 31) 的某一天的数字。



```javascript
let d = new Date();
let n = d.getDate();//返回月份的某一天
console.log(d);//Thu Dec 06 2018 10:26:48 GMT+0800 (中国标准时间)
console.log(n);//6
```



### getDay()


从 Date 对象返回一周中 (0 ~ 6) 的某一天的数字。



+ 注意： 星期天为 0, 星期一为 1, 以此类推。



```javascript
let d = new Date();
let n = d.getDay();//返回星期几
console.log(n);//4
```



### getHours()


返回 Date 对象的小时 (0 ~ 23)。



```javascript
let d = new Date();
let n = d.getHours();//返回小时
console.log(n);//10 - 以当时整理时间为准的
```



### getMinutes()


返回 Date 对象的分钟 (0 ~ 59)。



```javascript
let d = new Date();
let n = d.getMinutes();//返回分钟
console.log(n);//2 - 以当时整理时间为准的
```



### getSeconds()


返回 Date 对象的秒数 (0 ~ 59)。



```javascript
let d = new Date();
let n = d.getSeconds();//返回秒数
console.log(n);//11 - 以当时整理时间为准的
```



### getMilliseconds()


返回 Date 对象的毫秒(0 ~ 999)。



```javascript
let d = new Date();
let n = d.getMilliseconds();//返回毫秒数
console.log(n);//100 - 以当时整理时间为准的
```



### getTime()


返回 1970 年 1 月 1 日至今的毫秒数。



```javascript
let d = new Date();
let n = d.getTime();//返回距 1970 年 1 月 1 日之间的毫秒数
console.log(n);//1559786854978 - 以当时整理时间为准的
```



### parse(string)


返回1970年1月1日午夜到指定日期（字符串）的毫秒数。



+ 唯一一个参数（必选）表示日期和时间的字符串。



```javascript
let n = Date.parse("March 21, 2012");//返回1970/01/01至2012/3/21之间的毫秒数
console.log(n);//1332259200000
```



### getUTCFullYear()


根据世界时从 Date 对象返回四位数的年份。



+ 协调世界时 (UTC) 是以原子时秒长为基础，在时刻上尽量接近于世界时的一种时间计量系统。
+ 协调世界时，又称世界统一时间，世界标准时间，国际协调时间，简称UTC（ Universal Coordinated Time ) 。
+ UTC 时间类似于 GMT 时间；UTC 时间即是 GMT（格林尼治） 时间。



```javascript
let d = new Date();
let n = d.getUTCFullYear();
console.log(n);//2019
```



### getUTCMonth()


根据世界时从 Date 对象返回月份 (0 ~ 11)。



```javascript
let d = new Date();
let n = d.getUTCMonth();
console.log(n);//5
```



### getUTCDate()


根据世界时从 Date 对象返回月中的一天 (1 ~ 31)。



```javascript
let d = new Date();
let n = d.getUTCDate();
console.log(n);//6
```



### getUTCDay()


根据世界时从 Date 对象返回周中的一天 (0 ~ 6)。



```javascript
let d = new Date();
let n = d.getUTCDay();
console.log(n);//4
```



### getUTCHours()


根据世界时返回 Date 对象的小时 (0 ~ 23)。



```javascript
let d = new Date();
let n = d.getUTCHours();
console.log(n);//2 - 以当时整理时间为准的
```



### getUTCMinutes()


根据世界时返回 Date 对象的分钟 (0 ~ 59)。



```javascript
let d = new Date();
let n = d.getUTCMinutes();
console.log(n);//22 - 以当时整理时间为准的
```



### getUTCSeconds()


根据世界时返回 Date 对象的秒钟 (0 ~ 59)。



```javascript
let d = new Date();
let n = d.getUTCSeconds();
console.log(n);//26 - 以当时整理时间为准的
```



### getUTCMilliseconds()


根据世界时返回 Date 对象的毫秒(0 ~ 999)。



```javascript
let d = new Date();
let n = d.getUTCMilliseconds();
console.log(n);//651 - 以当时整理时间为准的
```



## 设置时间


### setFullYear(year,month,day)


设置 Date 对象中的年份（四位数字），返回1970年1月1日午夜至调整过日期的毫秒。



+ 第一个参数（必选）表示年份的四位整数。用本地时间表示。
+ 第二个参数（可选）表示月份的数值，用本地时间表示。介于 0 ~ 11 之间：-1 为去年的最后一个月、12 为明年的第一个月、13 为明年的第二个月
+ 第三个参数（可选）表示月中某一天的数值，用本地时间表示。介于 1 ~ 31 之间：0 为上个月最后一天、-1 为上个月最后一天之前的天数；如果当月有31天：32 为下个月的第一天；如果当月有30天：32 为下一个月的第二天。



```javascript
let d = new Date();
let n = d.setFullYear(2020);//设置年份为 2020
console.log(d);//Sat Jun 06 2020 10:26:48 GMT+0800 (中国标准时间)
console.log(n);//1591410524248
let m = d.setFullYear(2020, 10, 3);//把日期设置为 November 3, 2020
console.log(d);//Tue Nov 03 2020 10:37:00 GMT+0800 (中国标准时间)
console.log(m);//1557111215281
```



### setMonth(month,day)


设置 Date 对象中月份 (0 ~ 11)，返回1970年1月1日午夜至调整过日期的毫秒。



+ 第一个参数（必选）表示月份的数值，该值介于 0（一月） ~ 11（十二月） 之间：-1 为去年的最后一个月、12 为明年的第一个月、13 为明年的第二个月。
+ 第二个参数（可选）表示月的某一天的数值，该值介于 1 ~ 31 之间（以本地时间计）:0 为上个月的最后一天、-1 为上个月的最后一天之前的一天；如果当月有31天：32 为下个月的第一天；如果当月有30天：32 为下个月的第二天。



```javascript
let d = new Date();
let n = d.setMonth(4);//设置月份参数为 4 (5月份)
console.log(d);//Mon May 06 2019 10:57:03 GMT+0800 (中国标准时间)
console.log(n);//1557111698763
let m = d.setMonth(4, 20);//设置日期为4月20号
console.log(d);//Mon May 20 2019 10:57:03 GMT+0800 (中国标准时间)
console.log(m);//1558321392217
```



### setDate(day)


设置 Date 对象中月的某一天 (1 ~ 31)，返回1970年1月1日午夜至调整过日期的毫秒。



+ 唯一个一个（必选）参数表示一个月中的一天的一个数值（1 ~ 31）：0 为上一个月的最后一天、-1 为上一个月最后一天之前的一天；如果当月有 31 天：32 为下个月的第一天；如果当月有 30 天：32 为下一个月的第二天。



```javascript
let d = new Date();
let n = d.setDate(15);//设置一个月的某一天
console.log(d);//Sat Jun 15 2019 11:04:36 GMT+0800 (中国标准时间)
console.log(n);//1560568199819
```



### setTime(millisec)


以毫秒设置 Date 对象，返回参数 millisec。



```javascript
let d = new Date();
let n = d.setTime(1332403882588);//将向 1970年01月01日 添加 1332403882588毫秒，并显示新的日期和时间
console.log(d);//Thu Mar 22 2012 16:11:22 GMT+0800 (中国标准时间)
console.log(n);//1332403882588
```



### 其它


```javascript
setHours() - 设置 Date 对象中的小时 (0 ~ 23)。
setMinutes() - 设置 Date 对象中的分钟 (0 ~ 59)。
setSeconds() - 设置 Date 对象中的秒钟 (0 ~ 59)。
setMilliseconds() - 设置 Date 对象中的毫秒 (0 ~ 999)。
setUTCFullYear() - 根据世界时设置 Date 对象中的年份（四位数字）。
setUTCMonth() - 根据世界时设置 Date 对象中的月份 (0 ~ 11)。
setUTCDate() - 根据世界时设置 Date 对象中月份的一天 (1 ~ 31)。
setUTCHours() - 根据世界时设置 Date 对象中的小时 (0 ~ 23)。
setUTCMinutes() - 根据世界时设置 Date 对象中的分钟 (0 ~ 59)。
setUTCSeconds() - 根据世界时 (UTC) 设置指定时间的秒字段。
setUTCMilliseconds() - 根据世界时设置 Date 对象中的毫秒 (0 ~ 999)。
```



> 具体明细请看：[https://www.runoob.com/jsref/jsref-obj-date.html](https://www.runoob.com/jsref/jsref-obj-date.html)



## 转换字符串


### toString()


把 Date 对象转换为字符串。



```javascript
let d = new Date();
let n = d.toString();
console.log(n);//Thu Jun 06 2019 13:46:18 GMT+0800 (中国标准时间)
```



### toDateString()


把 Date 对象的日期部分转换为字符串。



```javascript
let d = new Date();
let n = d.toDateString();
console.log(n);//Thu Jun 06 2019
```



### toLocaleDateString()


根据本地时间格式，把 Date 对象的日期部分转换为字符串。



```javascript
let d = new Date();
let n = d.toLocaleDateString();
console.log(n);//2019-6-6
```



### toISOString()


使用 ISO 标准返回字符串的日期格式。



+ 该标准称为 ISO-8601 ，格式为: YYYY-MM-DDTHH:mm:ss.sssZ



```javascript
let d = new Date();
let n = d.toISOString();
console.log(n);//2019-06-06T05:42:52.528Z
```



### toJSON()


以 JSON 数据格式返回日期字符串。



+ JSON 数据用同样的格式就像x ISO-8601 标准: YYYY-MM-DDTHH:mm:ss.sssZ



```javascript
let d = new Date();
let n = d.toJSON();
console.log(n);//2019-06-06T05:44:32.863Z
```



### toTimeString()


把 Date 对象的时间部分转换为字符串。



```javascript
let d = new Date();
let n = d.toTimeString();
console.log(n);//13:50:22 GMT+0800 (中国标准时间)
```



### toLocaleTimeString()


根据本地时间格式，把 Date 对象的时间部分转换为字符串。



```javascript
let d = new Date();
let n = d.toLocaleTimeString();
console.log(n);//13:49:14
```



### toUTCString()


根据世界时 (UTC) ，把 Date 对象转换为字符串。



```javascript
let d = new Date();
let n = d.toUTCString();
console.log(n);//Thu, 06 Jun 2019 05:51:01 GMT
```



### UTC(year, month, day, hours, minutes, seconds, millisec)


根据世界时返回 1970 年 1 月 1 日 到指定日期的毫秒数。



+ year  必需。表示年份的四位数字。
+ month 必需。表示月份的整数，介于 0 ~ 11。
+ day   必需。表示日期的整数，介于 1 ~ 31。
+ hours 可选。表示小时的整数，介于 0 ~ 23。
+ minutes   可选。表示分钟的整数，介于 0 ~ 59。
+ seconds   可选。表示秒的整数，介于 0 ~ 59。
+ ms    可选。表示毫秒的整数，介于 0 ~ 999。



```javascript
let d = Date.UTC(2012, 2, 30);
console.log(d);//1333065600000
let D = new Date(Date.UTC(2012, 2, 30));
console.log(D);//Fri Mar 30 2012 08:00:00 GMT+0800 (中国标准时间)
```



### valueOf()


返回 Date 对象的原始值，原始值返回1970年1月1日午夜以来的毫秒数。



```javascript
let d = new Date();
let n = d.valueOf();
console.log(n);//1559800584306
```



## 扩展


> Date() 和 new Date() 区别



**当任意一个普通函数用于创建一类对象时，它就被称作构造函数，或构造器。**



+ new操作符来调用一个构造函数时,创建一个空对象obj
+ 将这个空对象的**proto**成员指向了构造函数对象的prototype成员对象
+ Date()就是一个静态方法、普通函数返回一个时间string作为普通函数的返回值



```javascript
typeof Number(10);//number
typeof new Number(10);//object
typeof Date();//string
typeof new Date();//object
let d1 = Date();//返回一个字符串（string），没有getDate等日期对象方法，内容为当前时间
let d2 = new Date();//返回一日期对象，可以调用getDate()，内容为当前时间
let d3 = Date("2000-1-1");//返回一个字符串（string），内容仍旧为当前时间，也就是不受参数影响
let d4 = new Date("2000-1-1");//返回一日期对象，可以调用getDate()，内容为2000年元旦
```





