---
title: JavaScript中RegExp对象整理
date: 2025-09-30
description: JavaScript中RegExp对象整理
tags: [Javascript, javascript]
categories: [Web前端, JavaScript]
---
# JavaScript中RegExp对象整理

## RegExp 对象


正则表达式是描述字符模式的对象。



正则表达式用于对字符串模式匹配及检索替换，是对字符串执行模式匹配的强大工具。



### 语法


```javascript
var patt=new RegExp(pattern,modifiers);
//或者更简单的方式:
var patt=/pattern/modifiers;
```



+ pattern（模式）描述了表达式的模式
+ modifiers（修饰符）用于指定全局匹配、区分大小写的匹配和多行匹配



> 注意：当使用构造函数创造正则对象时，需要常规的字符转义规则（在前面加反斜杠 \）。比如，以下是等价的：



```javascript
var re = new RegExp("\\w+");
var re = /\w+/;
```



### 修饰符


修饰符用于执行区分大小写和全局匹配:

| 修饰符 | 描述 |
| :--- | :--- |
| i | 执行对大小写不敏感的匹配。 |
| g | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |
| m | 执行多行匹配。 |




### 方括号


方括号用于查找某个范围内的字符：

| 表达式 | 描述 |
| :--- | :--- |
| [abc] | 查找方括号之间的任何字符。 |
| [^abc] | 查找任何不在方括号之间的字符。 |
| [0-9] | 查找任何从 0 至 9 的数字。 |
| [a-z] | 查找任何从小写 a 到小写 z 的字符。 |
| [A-Z] | 查找任何从大写 A 到大写 Z 的字符。 |
| [A-z] | 查找任何从大写 A 到小写 z 的字符。 |
| [adgk] | 查找给定集合内的任何字符 |
| [^adgk] | 查找给定集合外的任何字符。 |
| (red|blue|green) | 查找任何指定的选项。 |




### 元字符


元字符（Metacharacter）是拥有特殊含义的字符：

| 元字符 | 描述 |
| :--- | :--- |
| . | 查找单个字符，除了换行和行结束符。 |
| \w | 查找单词字符（a-z、A-Z、0-9，以及下划线, 包含 _ (下划线) 字符）。 |
| \W | 查找非单词字符。 |
| \d | 查找数字。 |
| \D | 查找非数字字符。 |
| \s | 查找空白字符。 |
| \S | 查找非空白字符。 |
| \b | 匹配单词边界。 |
| \B | 匹配非单词边界。 |
| \0 | 查找 NULL 字符。 |
| \n | 查找换行符。 |
| \f | 查找换页符。 |
| \r | 查找回车符。 |
| \t | 查找制表符。 |
| \v | 查找垂直制表符。 |
| \xxx | 查找以八进制数 xxx 规定的字符。 |
| \xdd | 查找以十六进制数 dd 规定的字符。 |
| \uxxxx | 查找以十六进制数 xxxx 规定的 Unicode 字符。 |




### 量词
| 量词 | 描述 |
| :--- | :--- |
| n+ | 匹配任何包含至少一个 n 的字符串。   例如，/a+/ 匹配 "candy" 中的 "a"，"caaaaaaandy" 中所有的 "a"。 |
| n* | 匹配任何包含零个或多个 n 的字符串。   例如，/bo*/ 匹配 "A ghost booooed" 中的 "boooo"，"A bird warbled" 中的 "b"，但是不匹配 "A goat grunted"。 |
| n? | 匹配任何包含零个或一个 n 的字符串。   例如，/e?le?/ 匹配 "angel" 中的 "el"，"angle" 中的 "le"。 |
| n{X} | 匹配包含 X 个 n 的序列的字符串。   例如，/a{2}/ 不匹配 "candy," 中的 "a"，但是匹配 "caandy," 中的两个 "a"，且匹配 "caaandy." 中的前两个 "a"。 |
| n{X,} | X 是一个正整数。前面的模式 n 连续出现至少 X 次时匹配。   例如，/a{2,}/ 不匹配 "candy" 中的 "a"，但是匹配 "caandy" 和 "caaaaaaandy." 中所有的 "a"。 |
| n{X,Y} | X 和 Y 为正整数。前面的模式 n 连续出现至少 X 次，至多 Y 次时匹配。   例如，/a{1,3}/ 不匹配 "cndy"，匹配 "candy," 中的 "a"，"caandy," 中的两个 "a"，匹配 "caaaaaaandy" 中的前面三个 "a"。   注意，当匹配 "caaaaaaandy" 时，即使原始字符串拥有更多的 "a"，匹配项也是 "aaa"。 |
| n$ | 匹配任何结尾为 n 的字符串。 |
| ^n | 匹配任何开头为 n 的字符串。 |
| ?=n | 匹配任何其后紧接指定字符串 n 的字符串。 |
| ?!n | 匹配任何其后没有紧接指定字符串 n 的字符串。 |




### RegExp 对象方法


#### exec()


检索字符串中的正则表达式的匹配，如果字符串中有匹配的值返回该匹配值，否则返回 null。



> 语法：RegExpObject.exec(string)



```javascript
let str = "Hello world!";
//查找"Hello"
let patt = /Hello/g;
let result = patt.exec(str);
console.log(result);//Hello
//查找 "RUNOOB"
patt = /RUNOOB/g;
result = patt.exec(str);
console.log(result);//null
```



#### test()


检测一个字符串是否匹配某个模式，如果字符串中有匹配的值返回 true ，否则返回 false。



> 语法：RegExpObject.test(string)



```javascript
let str = "Hello world!";
//查找"Hello"
let patt = /Hello/g;
let result = patt.test(str);//在字符串中全局搜索 "Hello" 字符串
console.log(result);//true
//查找 "Runoob"
patt = /Runoob/g;
result = patt.test(str);//在字符串中全局搜索 "Runoob" 字符串
console.log(result);//false
```



```javascript
//Javascript 判断是移动端浏览器还是 PC 端浏览器
if( /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) ) {
    document.write("移动")
} else {
    document.write("PC")
}
```



#### toString()


返回正则表达式的字符串值。



```javascript
let patt = new RegExp("RUNOOB", "g");
let res = patt.toString();
console.log(patt);///RUNOOB/g
console.log(res);///RUNOOB/g
```



详情请看[这里](https://www.runoob.com/jsref/jsref-obj-regexp.html)



