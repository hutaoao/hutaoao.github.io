---
title: JavaScript字符串方法整理-最齐全的
date: 2025-10-03
description: JavaScript字符串方法整理 - 最齐全的
tags: [Javascript, javascript]
categories: [Javascript]
---
# JavaScript字符串方法整理 - 最齐全的

### charAt(index)


返回在指定位置的字符（不改变原字符串）。



```javascript
let str = "HELLO WORLD";
let n = str.charAt(2);//返回字符串中的第三个字符:
console.log(str);//HELLO WORLD
console.log(n);//L
```



### charCodeAt(index)


返回在指定的位置的字符的 Unicode 编码（不改变原字符串）。



```javascript
let str = "HELLO WORLD";
let n = str.charCodeAt(0);//返回字符串第一个字符的 Unicode 编码
console.log(str);//HELLO WORLD
console.log(n);//72
```



### concat(string1, string2, ..., stringX)


连接两个或更多字符串，并返回新的字符串。



```javascript
let str1 = "Hello ";
let str2 = "world!";
let n = str1.concat(str2);//连接两个字符串
console.log(n);//Hello world!
```



### fromCharCode(code1, code2, ..., codeX)


将 Unicode 编码转为字符。



```javascript
let n = String.fromCharCode(65);
let m = String.fromCharCode(65, 66, 67);
console.log(n);//A
console.log(m);//ABC
```



### includes(searchString, start)


查找字符串中是否包含指定的子字符串，如果找到匹配的字符串则返回 true，否则返回 false。



第一个参数（必选）是要查找的字符串，第二个参数（可选）设置从那个位置开始查找，默认为 0。



```javascript
let str = "Hello world, welcome to the universe.";
let n = str.includes("world");//查找字符串是否包含 "world"
console.log(n);//true
let m = str.includes("world", 12);//从第 12 个索引位置开始查找字符串
console.log(m);//false
```



### indexOf(searchString, start)


返回某个指定的字符串值在字符串中首次出现的位置，如果没有找到匹配的字符串则返回 -1（不改变原字符串）。



第一个参数（必选）规定检索的字符串值；



第二个参数（可选）的整数。规定在字符串中开始检索的位置。它的合法取值是 0 到 string Object.length - 1。如省略该参数，则将从字符串的首字符开始检索。



```javascript
let str = "Hello world, welcome to the universe.";
let n = str.indexOf("welcome");//查找字符串 "welcome"
console.log(str);//Hello world, welcome to the universe.
console.log(n);//13
let m = str.indexOf("e", 5);
console.log(m);//14
```



### lastIndexOf(searchString, start)


从后向前搜索字符串，并从起始位置（0）开始计算返回字符串最后出现的位置，如果没有找到匹配字符串则返回 -1 。



第一个参数（必选）规定检索的字符串值；



第二个参数（可选）整数，规定在字符串中开始检索的位置。它的合法取值是 0 到 stringObject.length - 1。如省略该参数，则将从字符串的最后一个字符处开始检索。



```javascript
let str = "I am from runoob，welcome to runoob site.";
let n = str.lastIndexOf("runoob");//查找字符串 "runoob" 最后出现的位置
console.log(n);//28
let m = str.lastIndexOf("runoob", 20);//从第 20 个字符开始查找字符串 "runoob" 最后出现的位置
console.log(m);//10
let k = str.lastIndexOf("runoob", 9);//从第 10 个字符开始从后向前查找字符串 "runoob" 最后出现的位置
console.log(k);//-1
```



### match(regexp)


查找找到一个或多个正则表达式的匹配。



这个方法的行为在很大程度上有赖于 regexp 是否具有标志 g。如果 regexp 没有标志 g，那么 match() 方法就只能在 stringObject 中执行一次匹配。如果没有找到任何匹配的文本， match() 将返回 null。否则，它将返回一个数组，其中存放了与它找到的匹配文本有关的信息。



```javascript
let str = "The rain in SPAIN stays mainly in the plain";
let n = str.match(/ain/g);//在字符串中查找 "ain"
console.log(n);//[ 'ain', 'ain', 'ain' ]
let m = str.match(/ain/gi);//全局查找字符串 "ain"，且不区分大小写\
console.log(m);//[ 'ain', 'AIN', 'ain', 'ain' ]
```



### repeat(count)


复制字符串指定次数，并将它们连接在一起返回（不改变原字符串）。



```javascript
let str = "Runoob";
let n = str.repeat(2);//复制字符串 "Runoob" 两次
console.log(str);//Runoob
console.log(n);//RunoobRunoob
```



### replace(searchString, newvalue)


用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串，返回一个新的字符串。（不改变原字符串）。



第一个参数（必选）规定子字符串或要替换的模式的 RegExp 对象。请注意，如果该值是一个字符串，则将它作为要检索的直接量文本模式，而不是首先被转换为 RegExp 对象；



第二个参数（必选）一个字符串值。规定了替换文本或生成替换文本的函数。



```javascript
let str = "Visit Microsoft! Visit Microsoft!";
let n = str.replace("Microsoft", "Runoob");//在本例中，我们将执行一次替换，当第一个 "Microsoft" 被找到，它就被替换为 "Runoob"
console.log(str);//Visit Microsoft! Visit Microsoft!
console.log(n);//Visit Runoob! Visit Microsoft!
let str2="Mr Blue has a blue house and a blue car";
let m=str2.replace(/blue/g,"red");//执行一个全局替换
console.log(str2);//Mr Blue has a blue house and a blue car
console.log(m);//Mr Blue has a red house and a red car
let k=str2.replace(/blue/gi,"red");//执行一个全局替换
console.log(str2);//Mr Blue has a blue house and a blue car
console.log(k);//Mr red has a red house and a red car
```



### search(searchString)


检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，返回与指定查找的字符串或者正则表达式相匹配的 String 对象起始位置。如果没有找到任何匹配的子串，则返回 -1。（不改变原字符串）



```javascript
let str = "Visit Runoob!";
let n = str.search("Runoob");//查找 "Runoob"
console.log(str);//Visit Runoob!
console.log(n);//6
let str2="Mr. Blue has a blue house";
let m = str2.search("blue");//执行一次对大小写敏感的查找
console.log(str2);//Mr. Blue has a blue house
console.log(m);//15
let k = str2.search(/blue/i);//执行一次忽略大小写的检索
console.log(k);//4
```



### slice(start, end)


提取字符串的片断，并在新的字符串中返回被提取的部分。



使用 start（包含） 和 end（不包含） 参数来指定字符串提取的部分。



第一个参数（必选）要抽取的片断的起始下标；



第二个参数（可选）紧接着要截取的片段结尾的下标。若未指定此参数，则要提取的子串包括 start 到原字符串结尾的字符串。如果该参数是负数，那么它规定的是从字符串的尾部开始算起的位置。



```javascript
let str = "Hello world!";
let n = str.slice(3);//从字符串的第3个位置提取字符串片段
console.log(str);//Hello world!
console.log(n);//lo world!
let m=str.slice(3,8);//从字符串的第3个位置到第8个位置直接的字符串片段
console.log(m);//lo wo
let k = str.slice(-1);
console.log(k);//!
let l = str.slice(-3,-1);
console.log(l);//ld
```



### split(separator, limit)


把字符串分割为字符串数组（不改变原始字符串）。



注意：如果把空字符串 ("") 用作 separator，那么 stringObject 中的每个字符之间都会被分割。



第一个参数（可选）字符串或正则表达式，从该参数指定的地方分割 string Object；



第二个参数（可选）该参数可指定返回的数组的最大长度。如果设置了该参数，返回的子串不会多于这个参数指定的数组。如果没有设置该参数，整个字符串都会被分割，不考虑它的长度。



```javascript
let str = "How are you doing today?";
let n = str.split(" ");//把一个字符串分割成字符串数组
let m = str.split("");
console.log(str);//How are you doing today?
console.log(n);//[ 'How', 'are', 'you', 'doing', 'today?' ]
console.log(m);//[ 'H','o','w', ' ','a','r','e',' ','y','o','u',' ','d','o','i','n','g',' ','t','o','d','a','y','?' ]
let k = str.split();//省略分割参数
console.log(k);//[ 'How are you doing today?' ]
let l=str.split(" ",3);//使用 limit 参数
console.log(l);//[ 'How', 'are', 'you' ]
let j = str.split("o");//使用一个字符作为分隔符
console.log(j);//[ 'H', 'w are y', 'u d', 'ing t', 'day?' ]
```



### startsWith(searchString, start)


查看字符串是否以指定的子字符串开头，如果是以指定的子字符串开头返回 true，否则 false。



startsWith() 方法对大小写敏感。



第一个参数（必选）要查找的字符串；第二个参数（可选）查找的开始位置，默认是0。



```javascript
let str = "Hello world, welcome to the Runoob.";
let n = str.startsWith("Hello");//查看字符串是否为 "Hello" 开头
console.log(n);//true
let m = str.startsWith("world", 6);
console.log(m);//true
```



### substr(start, length)


从起始索引号提取字符串中指定数目的字符（不改变原字符串）。



第一个参数（必选）要抽取的子串的起始下标。必须是数值。如果是负数，那么该参数声明从字符串的尾部开始算起的位置。也就是说，-1 指字符串中最后一个字符，-2 指倒数第二个字符，以此类推；



第二个参数（可选）字符串中的字符数。必须是数值。如果省略了该参数，那么返回从 stringObject 的开始位置到结尾的字串。



```javascript
let str = "Hello world!";
let n = str.substr(2,3);//抽取指定数目的字符
console.log(str);//Hello world!
console.log(n);//llo
let m = str.substr(2);//从字符串第二个位置中提取一些字符
console.log(m);//llo world!
```



### substring(from, to)


提取字符串中两个指定的索引号之间的字符，返回的子串包括 开始 处的字符，但不包括 结束 处的字符。



第一个参数（必选）一个非负的整数，规定要提取的子串的第一个字符在 string Object 中的位置；



第二个参数（可选）一个非负的整数，比要提取的子串的最后一个字符在 string Object 中的位置多 1。如果省略该参数，那么返回的子串会一直到字符串的结尾。



```javascript
let str = "Hello world!";
let n = str.substring(3);
let m = str.substring(3,7);
console.log(n);//lo world!
console.log(m);//lo w
```



### toLowerCase()


把字符串转换为小写（不改变原字符串）。



```javascript
let str = "Runoob";
let n = str.toLowerCase();
console.log(str);//Runoob
console.log(n);//runoob
```



### toUpperCase()


把字符串转换为大写（不改变原字符串）。



```javascript
let str = "Runoob";
let n = str.toUpperCase();
console.log(str);//Runoob
console.log(n);//RUNOOB
```



### trim()


去除字符串两边的空白，用于删除字符串的头尾空格（不改变原字符串）。



```javascript
let str = "       Run  oob        ";
let n = str.trim();
console.log(str);//       Run  oob
console.log(n);//Run  oob
```



### toString()


返回一个字符串。



```javascript
let str = "Runoob";
let n = str.toString();
console.log(n);//Runoob
```



### valueOf()


返回某个字符串对象的原始值，通常由 JavaScript 在后台自动进行调用，而不是显式地处于代码中。



```javascript
let str = "Hello world!";
let n = str.valueOf();
console.log(n);//Hello world!
```



### toLocaleLowerCase()


根据本地主机的语言环境把字符串转换为小写（不改变原字符串）。



通常，该方法与 toLowerCase() 方法返回的结果相同，只有几种语言（如土耳其语）具有地方特有的大小写映射。



```javascript
let str = "Runoob";
let n = str.toLocaleLowerCase();
console.log(str);//Runoob
console.log(n);//runoob
```



### toLocaleUpperCase()


根据本地主机的语言环境把字符串转换为大写（不改变原字符串）。



通常，该方法与 toUpperCase() 方法返回的结果相同，只有几种语言（如土耳其语）具有地方特有的大小写映射。



```javascript
let str = "Runoob";
let n = str.toLocaleUpperCase();
console.log(str);//Runoob
console.log(n);//RUNOOB
```



### 扩展


1. big()    用大号字体显示字符串。
2. blink()  显示闪动字符串。
3. bold()   使用粗体显示字符串。
4. fixed()  以打字机文本显示字符串。
5. fontcolor()  使用指定的颜色来显示字符串。
6. fontsize()   使用指定的尺寸来显示字符串。
7. italics()    使用斜体显示字符串。
8. link()   将字符串显示为链接。
9. small()  使用小字号来显示字符串。
10. strike()    用于显示加删除线的字符串。
11. sub()   把字符串显示为下标。
12. sup()   把字符串显示为上标。



```html
<!DOCTYPE html>
<html>
    <head> 
        <meta charset="utf-8"> 
        <title>菜鸟教程(runoob.com)</title> 
    </head>
    <body>
        <script>
            var txt = "Hello World!";
            document.write("<p>字体变大: " + txt.big() + "</p>");
            document.write("<p>字体缩小: " + txt.small() + "</p>");
            document.write("<p>字体加粗: " + txt.bold() + "</p>");
            document.write("<p>斜体: " + txt.italics() + "</p>");
            document.write("<p>固定定位: " + txt.fixed() + "</p>");
            document.write("<p>加删除线: " + txt.strike() + "</p>");
            document.write("<p>字体颜色: " + txt.fontcolor("green") + "</p>");
            document.write("<p>字体大小: " + txt.fontsize(6) + "</p>");
            document.write("<p>下标: " + txt.sub() + "</p>");
            document.write("<p>上标: " + txt.sup() + "</p>");
            document.write("<p>链接: " + txt.link("http://www.w3cschool.cc") + "</p>");
            document.write("<p>闪动文本: " + txt.blink() + " (不能用于IE,Chrome,或者Safari)</p>");
        </script>
    </body>
</html>
```





