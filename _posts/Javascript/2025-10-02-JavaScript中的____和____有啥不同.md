---
title: JavaScript中的____和____有啥不同
date: 2025-10-02
description: JavaScript 中的"??"和"||" 有啥不同
tags: [Javascript, javascript]
categories: [Javascript]
---
# JavaScript 中的"??"和"||" 有啥不同

<font style="color:rgb(51, 51, 51);">在 JavaScript 开发中，很多小伙伴都会遇到一个场景，就是要给变量设置一个默认值，比如当变量没有有效值时，使用一个备用值。这个时候，可能有两个操作符会让你感到困惑：</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);">（空值合并运算符）和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);">（逻辑或运算符）。一开始看，它们似乎都能达到相同的效果，但其实它们背后的逻辑完全不同，适用的场景也不一样。今天我们就来聊聊这两者的区别，帮你快速上手，避免掉坑！</font>

## **<font style="color:rgb(0, 0, 0);">"||" 是怎么工作的？—— 就像找备胎一样！</font>**

<code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 就好像是在说：“如果这个不行，就换另一个。”它的具体作用是检查左边的值（第一个计划），如果左边这个值是 </font>**<font style="color:rgb(51, 51, 51);">“假”</font>**<font style="color:rgb(51, 51, 51);">（也就是不靠谱、不能用的意思），它就会去看右边的值（第二个计划），并返回右边的值。</font>

<font style="color:rgb(51, 51, 51);">举个例子来理解一下：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
let result = value1 || value2;
```

<font style="color:rgb(51, 51, 51);">这段代码就是在说：“如果 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value1</font></code><font style="color:rgb(51, 51, 51);"> 能用，就用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value1</font></code><font style="color:rgb(51, 51, 51);">；如果 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value1</font></code><font style="color:rgb(51, 51, 51);"> 不行，那就用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value2</font></code><font style="color:rgb(51, 51, 51);"> 吧！”那问题来了，什么情况下 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value1</font></code><font style="color:rgb(51, 51, 51);"> 会不行呢？</font>

### **<font style="color:rgb(0, 0, 0);">什么是 "假值"？</font>**

<font style="color:rgb(51, 51, 51);">在 JavaScript 里，有一些特殊的值会被认为是“假”的，像这些：</font>

* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);">（假）</font>
* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">（零）</font>
* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">""</font></code><font style="color:rgb(51, 51, 51);">（空字符串）</font>
* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);">（表示空）</font>
* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);">（未定义）</font>
* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">NaN</font></code><font style="color:rgb(51, 51, 51);">（非数字）</font>

<font style="color:rgb(51, 51, 51);">这些值都被认为是不能用的“假值”。如果你遇到这些值，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 操作符就会帮你返回后面的备用计划。</font>

### **<font style="color:rgb(0, 0, 0);">举个栗子</font>****<font style="color:rgb(0, 0, 0);">🌰</font>****<font style="color:rgb(0, 0, 0);">：</font>**

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
let username = "" || "游客"; // 返回 "游客"
let score = 0 || 10;          // 返回 10
```

<font style="color:rgb(51, 51, 51);">在上面的例子里，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">""</font></code><font style="color:rgb(51, 51, 51);">（空字符串）和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);"> 都是“假值”，所以 JavaScript 会忽略它们，直接选择后面的“游客”和 10 作为返回值。</font>

### **<font style="color:rgb(0, 0, 0);">那什么时候用 "||" 呢？</font>**

<code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 的最佳使用场景就是当你不确定一个值是否靠谱的时候，你可以为它准备一个备用值。就像生活中你遇到的两手准备：如果第一种方案失败了，立刻执行第二种。</font>

<font style="color:rgb(51, 51, 51);">比如你想让用户输入他们的名字，但如果用户没有输入名字，你就让系统自动叫他们“游客”。用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 来实现这个需求简直再好不过了：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
function greetUser(name) {
  let username = name || "游客";
  console.log(`你好, ${username}!`);
}

greetUser("");  // 输出: 你好, 游客!
greetUser("Alice");  // 输出: 你好, Alice!
```

<font style="color:rgb(51, 51, 51);">在这个例子里，如果用户没有输入名字（比如空字符串 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">""</font></code><font style="color:rgb(51, 51, 51);">），我们就用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 返回一个默认值“游客”。但如果用户输入了名字（比如 “Alice”），我们就用这个名字打招呼。</font>

## **<font style="color:rgb(0, 0, 0);">JavaScript 中的"??"操作符：只关心空值，别搞混了！</font>**

<font style="color:rgb(51, 51, 51);">JavaScript 里的</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);">（空值合并运算符）看起来和我们之前聊过的</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);">有点像，但它其实更“挑剔”！如果说</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);">是遇到假值就找备胎，那么</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);">只会在 </font><code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font>**</code><font style="color:rgb(51, 51, 51);"> 和 </font><code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font>**</code><font style="color:rgb(51, 51, 51);"> 出现时才出手救场。今天咱们就来深入了解一下这个 ECMAScript 2020 (ES11) 中的新工具。</font>

### **<font style="color:rgb(0, 0, 0);">"??" 是怎么工作的？—— 只处理“真正”的空值</font>**

<code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 操作符的工作方式是：当左边的值是 </font><code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font>**</code><font style="color:rgb(51, 51, 51);"> 或 </font><code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font>**</code><font style="color:rgb(51, 51, 51);"> 时，才返回右边的备用值。像 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">""</font></code><font style="color:rgb(51, 51, 51);"> 这些“假值”，在</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);">眼里都是 </font>**<font style="color:rgb(51, 51, 51);">有效值</font>**<font style="color:rgb(51, 51, 51);">，它们不会触发备用计划。</font>

<font style="color:rgb(51, 51, 51);">语法很简单：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
let result = value1 ?? value2;
```

<font style="color:rgb(51, 51, 51);">意思是：如果 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value1</font></code><font style="color:rgb(51, 51, 51);"> 是 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 或 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);">，那就返回 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value2</font></code><font style="color:rgb(51, 51, 51);">；否则就直接用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value1</font></code><font style="color:rgb(51, 51, 51);">。</font>

### **<font style="color:rgb(0, 0, 0);">举个例子</font>****<font style="color:rgb(0, 0, 0);">🌰</font>****<font style="color:rgb(0, 0, 0);">：</font>**

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
let username = "" ?? "游客"; // 返回 ""
let score = 0 ?? 10;          // 返回 0
```

<font style="color:rgb(51, 51, 51);">在这两个例子中，空字符串 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">""</font></code><font style="color:rgb(51, 51, 51);"> 和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);"> 虽然是“假值”，但因为它们不是 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 或 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);">，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 不会认为它们无效，所以直接返回它们本身。这样就避免了 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 操作符那种“误判”的情况——如果你真的想用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);"> 或 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">""</font></code><font style="color:rgb(51, 51, 51);"> 作为有效值，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 就非常合适。</font>

### **<font style="color:rgb(0, 0, 0);">什么时候用"??"？</font>**

<code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 非常适合在你 </font>**<font style="color:rgb(51, 51, 51);">只想处理</font>**<code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font>**</code>**<font style="color:rgb(51, 51, 51);">或</font>**<code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font>**</code><font style="color:rgb(51, 51, 51);"> 的场景下使用。比方说，你希望某个变量可以是 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);"> 或 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);"> 这样的值，而不想让它们触发默认值。来看个小例子：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
function getScore(score) {
  return score ?? 10;
}

console.log(getScore(0));    // 输出: 0
console.log(getScore(null)); // 输出: 10
```

<font style="color:rgb(51, 51, 51);">在这个例子里，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">score</font></code><font style="color:rgb(51, 51, 51);"> 可以是 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">，也可以是 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);">。如果传入 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 不会把它当成空值，而是直接返回 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">。但如果 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">score</font></code><font style="color:rgb(51, 51, 51);"> 是 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);">，它就会返回默认值 10。这种行为对于某些场景非常有用，比如分数为 0 的时候你不希望它被误当成无效值。</font>

## <code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font>**</code>**<font style="color:rgb(0, 0, 0);"> 和 </font>**<code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font>**</code>**<font style="color:rgb(0, 0, 0);"> 的关键区别：用错容易踩坑哦！</font>**

<font style="color:rgb(51, 51, 51);">在 JavaScript 里，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);">（空值合并运算符）和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);">（逻辑或运算符）都是用来设置默认值的利器，初学者可能觉得它们差不多，但其实它们的行为有很大不同。为了避免代码里的坑，我们必须清楚两者的使用场景和差异。</font>

### **<font style="color:rgb(0, 0, 0);">1. "Falsy" vs. "Nullish"：谁才是真的空？</font>**

<code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);">：它的逻辑是，遇到任何“假值”（falsy）就会返回备用值。这些“假值”包括：</font>

* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code>
* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code>
* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">""</font></code><font style="color:rgb(51, 51, 51);">（空字符串）</font>
* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code>
* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code>
* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">NaN</font></code><font style="color:rgb(51, 51, 51);">（非数字）</font>

<font style="color:rgb(51, 51, 51);">只要值是这些“假值”，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 就会触发备用值。</font>

<code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);">：相比之下，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 只关注 </font><code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font>**</code><font style="color:rgb(51, 51, 51);"> 和</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);">。也就是说，像 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);"> 和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">""</font></code><font style="color:rgb(51, 51, 51);"> 这些虽然看似“假”的值，但在 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 眼中是有效的，不会触发备用值。</font>

### **<font style="color:rgb(0, 0, 0);">2. 使用场景：该选谁？</font>**

* <font style="color:rgb(51, 51, 51);">使用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);">：当你想对所有“假值”都提供一个备用方案时，比如 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);"> 或空字符串。这种情况下，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 是你最好的选择。</font>
* <font style="color:rgb(51, 51, 51);">使用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);">：如果你只想处理 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);">，而对 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);"> 这些值保留，那么 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 更适合你。</font>

### **<font style="color:rgb(0, 0, 0);">3. 举例对比：看代码更直观</font>**

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
let value1 = 0 || 5;    // value1 是 5
let value2 = 0 ?? 5;    // value2 是 0

let value3 = null || "默认值";  // value3 是 "默认值"
let value4 = null ?? "默认值";  // value4 是 "默认值"
```

<font style="color:rgb(51, 51, 51);">在第一个例子中，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value1</font></code><font style="color:rgb(51, 51, 51);"> 变成了 5，因为 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);"> 被 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 认为是“假值”。而在 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 的情况下，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value2</font></code><font style="color:rgb(51, 51, 51);"> 保持为 0，因为 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);"> 并不是 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 或 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);">，所以不会触发备用值。</font>

<font style="color:rgb(51, 51, 51);">第二个例子里，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 在两种操作符中都触发了备用值，结果一样。</font>

### **<font style="color:rgb(0, 0, 0);">4. 潜在的 Bug：小心别踩坑！</font>**

<font style="color:rgb(51, 51, 51);">误用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 会导致一些你没料到的问题，特别是当 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);"> 或 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">""</font></code><font style="color:rgb(51, 51, 51);"> 是有效值时。来看个例子：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
function getDiscountedPrice(price) {
  return price || 100;  // 这里会返回 100，即使 price 是 0
}

console.log(getDiscountedPrice(0));  // 输出: 100（错误）
```

<font style="color:rgb(51, 51, 51);">这里的逻辑会导致问题，因为 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);"> 是一个有效的价格，但由于 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 把它当作“假值”，最终返回了备用值 100。解决方案就是用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);">，这样 0 就不会被误判了：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
function getDiscountedPrice(price) {
  return price ?? 100;  // 这里会正确返回 0
}

console.log(getDiscountedPrice(0));  // 输出: 0（正确）
```

### **<font style="color:rgb(0, 0, 0);">5. 短路求值：谁先说了算？</font>**

<code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 都使用了 </font>**<font style="color:rgb(51, 51, 51);">短路求值</font>**<font style="color:rgb(51, 51, 51);">，意思是如果左边的值能决定结果，右边的值就不会被计算。但两者的判断标准不同——</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 会在遇到任意“假值”时短路，而 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 只会在 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 或 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);"> 时短路。</font>

### **<font style="color:rgb(0, 0, 0);">6.总结：怎么选？</font>**

* <font style="color:rgb(51, 51, 51);">如果你需要处理所有的“假值”（包括 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">""</font></code><font style="color:rgb(51, 51, 51);">），就用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);">。</font>
* <font style="color:rgb(51, 51, 51);">如果你只关心 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);">，而 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);"> 这些值也是有效输入，那就用 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);">。</font>

## **<font style="color:rgb(0, 0, 0);">结合 </font>**<code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font>**</code>**<font style="color:rgb(0, 0, 0);"> 和 </font>**<code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font>**</code>**<font style="color:rgb(0, 0, 0);">：多重保险，让代码更健壮！</font>**

<font style="color:rgb(51, 51, 51);">有时候，你可能会遇到更复杂的场景，既要处理 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);">，又需要对其他“假值”如 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">""</font></code><font style="color:rgb(51, 51, 51);"> 做特殊处理。这种情况下，我们可以把 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 结合起来，打造一个更加全面的“多重保险”逻辑。</font>

### **<font style="color:rgb(0, 0, 0);">为什么要同时用 </font>**<code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font>**</code>**<font style="color:rgb(0, 0, 0);"> 和 </font>**<code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font>**</code>**<font style="color:rgb(0, 0, 0);">？</font>**

<code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 负责处理 </font><code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font>**</code><font style="color:rgb(51, 51, 51);"> 和 **</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);">**，而 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 可以处理 </font>**<font style="color:rgb(51, 51, 51);">所有“假值”</font>**<font style="color:rgb(51, 51, 51);">（如 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);">、空字符串等）。有些情况下，你可能希望 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);"> 返回默认值，而对于其他“假值”则使用不同的逻辑处理。</font>

<font style="color:rgb(51, 51, 51);">来看个例子：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
let result = (value ?? defaultValue) || fallbackValue;
```

<font style="color:rgb(51, 51, 51);">在这段代码中，我们做了两步处理：</font>

<font style="color:rgb(51, 51, 51);">1.</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value ?? defaultValue</font></code><font style="color:rgb(51, 51, 51);">：首先，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 检查 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value</font></code><font style="color:rgb(51, 51, 51);"> 是否为 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 或 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);">。如果是，那么返回 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">defaultValue</font></code><font style="color:rgb(51, 51, 51);">；如果不是，则直接返回 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value</font></code><font style="color:rgb(51, 51, 51);">。2. </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">|| fallbackValue</font></code><font style="color:rgb(51, 51, 51);">：接下来，结果再经过 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);">，如果这个结果是“假值”（例如 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);">、空字符串等），就返回 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">fallbackValue</font></code><font style="color:rgb(51, 51, 51);">。</font>

##### **<font style="color:rgb(0, 0, 0);">举个栗子</font>\*\*\*\*<font style="color:rgb(0, 0, 0);">🌰</font>**

<font style="color:rgb(51, 51, 51);">假设你在处理一个分数系统，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 或 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);"> 的时候要用默认分数，但同时如果分数是 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);"> 也不能直接忽略，需要特殊处理。我们可以这样写：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
function getFinalScore(score) {
  return (score ?? 50) || "分数无效";
}

console.log(getFinalScore(null));  // 输出: 50（null 被 `??` 处理）
console.log(getFinalScore(0));     // 输出: "分数无效"（0 被 `||` 处理）
console.log(getFinalScore(80));    // 输出: 80（有效分数）
```

* <code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font>**</code>**<font style="color:rgb(51, 51, 51);"> 的情况</font>**<font style="color:rgb(51, 51, 51);">：</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">score</font></code><font style="color:rgb(51, 51, 51);"> 是 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 时，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 返回默认值 50，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 不会再触发。</font>
* <code>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font>**</code>**<font style="color:rgb(51, 51, 51);"> 的情况</font>**<font style="color:rgb(51, 51, 51);">：</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">score</font></code><font style="color:rgb(51, 51, 51);"> 是 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);"> 时，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 不起作用，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 将 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);"> 视为“假值”，返回 "分数无效"。</font>
* **<font style="color:rgb(51, 51, 51);">有效分数的情况</font>**<font style="color:rgb(51, 51, 51);">：</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">score</font></code><font style="color:rgb(51, 51, 51);"> 是 80 时，直接返回 80。</font>

### **<font style="color:rgb(0, 0, 0);">什么时候使用这种组合？</font>**

<font style="color:rgb(51, 51, 51);">这种组合特别适合需要处理复杂逻辑的场景，比如：</font>

* <code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 或 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);"> 的时候，返回一个默认值；</font>
* <font style="color:rgb(51, 51, 51);">其他“假值”（如 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);">）也需要单独处理，避免被误认为无效。</font>

## **<font style="color:rgb(0, 0, 0);">结束</font>**

<font style="color:rgb(51, 51, 51);">在 JavaScript 开发中，</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 绝对是处理默认值的利器，虽然它们看上去很像，但实际应用中却有明显区别。</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">||</font></code><font style="color:rgb(51, 51, 51);"> 会把很多值当作“假值”，包括 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">0</font></code><font style="color:rgb(51, 51, 51);">、</font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">false</font></code><font style="color:rgb(51, 51, 51);">、空字符串等；而 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">??</font></code><font style="color:rgb(51, 51, 51);"> 只关心 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">null</font></code><font style="color:rgb(51, 51, 51);"> 和 </font><code><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">undefined</font></code><font style="color:rgb(51, 51, 51);">。因此，合理选择这两个运算符，能让你避免不必要的 Bug，尤其是在处理特殊值的时候。</font>


> 更新: 2025-01-10 14:31:56  
> 原文: <https://www.yuque.com/hutaoao/blog/kzr0d2e567fhqrkl>