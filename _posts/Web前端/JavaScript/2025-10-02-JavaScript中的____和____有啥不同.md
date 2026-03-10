---
title: JavaScript中的____和____有啥不同
date: 2025-10-02
description: JavaScript 中的"??"和"||" 有啥不同
tags: [javascript]
categories: [Web前端, JavaScript]
---
# JavaScript 中的"??"和"||" 有啥不同

在 JavaScript 开发中，很多小伙伴都会遇到一个场景，就是要给变量设置一个默认值，比如当变量没有有效值时，使用一个备用值。这个时候，可能有两个操作符会让你感到困惑：<code>??</code>（空值合并运算符）和 <code>||</code>（逻辑或运算符）。一开始看，它们似乎都能达到相同的效果，但其实它们背后的逻辑完全不同，适用的场景也不一样。今天我们就来聊聊这两者的区别，帮你快速上手，避免掉坑！

## **"||" 是怎么工作的？—— 就像找备胎一样！**

<code>||</code> 就好像是在说：“如果这个不行，就换另一个。”它的具体作用是检查左边的值（第一个计划），如果左边这个值是 **“假”**（也就是不靠谱、不能用的意思），它就会去看右边的值（第二个计划），并返回右边的值。

举个例子来理解一下：

**代码语言：**javascript

**复制**

```javascript
let result = value1 || value2;
```

这段代码就是在说：“如果 <code>value1</code> 能用，就用 <code>value1</code>；如果 <code>value1</code> 不行，那就用 <code>value2</code> 吧！”那问题来了，什么情况下 <code>value1</code> 会不行呢？

### **什么是 "假值"？**

在 JavaScript 里，有一些特殊的值会被认为是“假”的，像这些：

* <code>false</code>（假）
* <code>0</code>（零）
* <code>""</code>（空字符串）
* <code>null</code>（表示空）
* <code>undefined</code>（未定义）
* <code>NaN</code>（非数字）

这些值都被认为是不能用的“假值”。如果你遇到这些值，<code>||</code> 操作符就会帮你返回后面的备用计划。

### **举个栗子****🌰****：**

**代码语言：**javascript

**复制**

```javascript
let username = "" || "游客"; // 返回 "游客"
let score = 0 || 10;          // 返回 10
```

在上面的例子里，<code>""</code>（空字符串）和 <code>0</code> 都是“假值”，所以 JavaScript 会忽略它们，直接选择后面的“游客”和 10 作为返回值。

### **那什么时候用 "||" 呢？**

<code>||</code> 的最佳使用场景就是当你不确定一个值是否靠谱的时候，你可以为它准备一个备用值。就像生活中你遇到的两手准备：如果第一种方案失败了，立刻执行第二种。

比如你想让用户输入他们的名字，但如果用户没有输入名字，你就让系统自动叫他们“游客”。用 <code>||</code> 来实现这个需求简直再好不过了：

**代码语言：**javascript

**复制**

```javascript
function greetUser(name) {
  let username = name || "游客";
  console.log(`你好, ${username}!`);
}

greetUser("");  // 输出: 你好, 游客!
greetUser("Alice");  // 输出: 你好, Alice!
```

在这个例子里，如果用户没有输入名字（比如空字符串 <code>""</code>），我们就用 <code>||</code> 返回一个默认值“游客”。但如果用户输入了名字（比如 “Alice”），我们就用这个名字打招呼。

## **JavaScript 中的"??"操作符：只关心空值，别搞混了！**

JavaScript 里的<code>??</code>（空值合并运算符）看起来和我们之前聊过的<code>||</code>有点像，但它其实更“挑剔”！如果说<code>||</code>是遇到假值就找备胎，那么<code>??</code>只会在 <code>**null**</code> 和 <code>**undefined**</code> 出现时才出手救场。今天咱们就来深入了解一下这个 ECMAScript 2020 (ES11) 中的新工具。

### **"??" 是怎么工作的？—— 只处理“真正”的空值**

<code>??</code> 操作符的工作方式是：当左边的值是 <code>**null**</code> 或 <code>**undefined**</code> 时，才返回右边的备用值。像 <code>0</code>、<code>false</code>、<code>""</code> 这些“假值”，在<code>??</code>眼里都是 **有效值**，它们不会触发备用计划。

语法很简单：

**代码语言：**javascript

**复制**

```javascript
let result = value1 ?? value2;
```

意思是：如果 <code>value1</code> 是 <code>null</code> 或 <code>undefined</code>，那就返回 <code>value2</code>；否则就直接用 <code>value1</code>。

### **举个例子****🌰****：**

**代码语言：**javascript

**复制**

```javascript
let username = "" ?? "游客"; // 返回 ""
let score = 0 ?? 10;          // 返回 0
```

在这两个例子中，空字符串 <code>""</code> 和 <code>0</code> 虽然是“假值”，但因为它们不是 <code>null</code> 或 <code>undefined</code>，<code>??</code> 不会认为它们无效，所以直接返回它们本身。这样就避免了 <code>||</code> 操作符那种“误判”的情况——如果你真的想用 <code>0</code> 或 <code>""</code> 作为有效值，<code>??</code> 就非常合适。

### **什么时候用"??"？**

<code>??</code> 非常适合在你 **只想处理**<code>**null**</code>**或**<code>**undefined**</code> 的场景下使用。比方说，你希望某个变量可以是 <code>0</code> 或 <code>false</code> 这样的值，而不想让它们触发默认值。来看个小例子：

**代码语言：**javascript

**复制**

```javascript
function getScore(score) {
  return score ?? 10;
}

console.log(getScore(0));    // 输出: 0
console.log(getScore(null)); // 输出: 10
```

在这个例子里，<code>score</code> 可以是 <code>0</code>，也可以是 <code>null</code>。如果传入 <code>0</code>，<code>??</code> 不会把它当成空值，而是直接返回 <code>0</code>。但如果 <code>score</code> 是 <code>null</code>，它就会返回默认值 10。这种行为对于某些场景非常有用，比如分数为 0 的时候你不希望它被误当成无效值。

## <code>**??**</code>** 和 **<code>**||**</code>** 的关键区别：用错容易踩坑哦！**

在 JavaScript 里，<code>??</code>（空值合并运算符）和 <code>||</code>（逻辑或运算符）都是用来设置默认值的利器，初学者可能觉得它们差不多，但其实它们的行为有很大不同。为了避免代码里的坑，我们必须清楚两者的使用场景和差异。

### **1. "Falsy" vs. "Nullish"：谁才是真的空？**

<code>||</code>：它的逻辑是，遇到任何“假值”（falsy）就会返回备用值。这些“假值”包括：

* <code>false</code>
* <code>0</code>
* <code>""</code>（空字符串）
* <code>null</code>
* <code>undefined</code>
* <code>NaN</code>（非数字）

只要值是这些“假值”，<code>||</code> 就会触发备用值。

<code>??</code>：相比之下，<code>??</code> 只关注 <code>**null**</code> 和<code>undefined</code>。也就是说，像 <code>0</code>、<code>false</code> 和 <code>""</code> 这些虽然看似“假”的值，但在 <code>??</code> 眼中是有效的，不会触发备用值。

### **2. 使用场景：该选谁？**

* 使用 <code>||</code>：当你想对所有“假值”都提供一个备用方案时，比如 <code>false</code>、<code>0</code> 或空字符串。这种情况下，<code>||</code> 是你最好的选择。
* 使用 <code>??</code>：如果你只想处理 <code>null</code> 和 <code>undefined</code>，而对 <code>0</code>、<code>false</code> 这些值保留，那么 <code>??</code> 更适合你。

### **3. 举例对比：看代码更直观**

**代码语言：**javascript

**复制**

```javascript
let value1 = 0 || 5;    // value1 是 5
let value2 = 0 ?? 5;    // value2 是 0

let value3 = null || "默认值";  // value3 是 "默认值"
let value4 = null ?? "默认值";  // value4 是 "默认值"
```

在第一个例子中，<code>value1</code> 变成了 5，因为 <code>0</code> 被 <code>||</code> 认为是“假值”。而在 <code>??</code> 的情况下，<code>value2</code> 保持为 0，因为 <code>0</code> 并不是 <code>null</code> 或 <code>undefined</code>，所以不会触发备用值。

第二个例子里，<code>null</code> 在两种操作符中都触发了备用值，结果一样。

### **4. 潜在的 Bug：小心别踩坑！**

误用 <code>||</code> 会导致一些你没料到的问题，特别是当 <code>0</code>、<code>false</code> 或 <code>""</code> 是有效值时。来看个例子：

**代码语言：**javascript

**复制**

```javascript
function getDiscountedPrice(price) {
  return price || 100;  // 这里会返回 100，即使 price 是 0
}

console.log(getDiscountedPrice(0));  // 输出: 100（错误）
```

这里的逻辑会导致问题，因为 <code>0</code> 是一个有效的价格，但由于 <code>||</code> 把它当作“假值”，最终返回了备用值 100。解决方案就是用 <code>??</code>，这样 0 就不会被误判了：

**代码语言：**javascript

**复制**

```javascript
function getDiscountedPrice(price) {
  return price ?? 100;  // 这里会正确返回 0
}

console.log(getDiscountedPrice(0));  // 输出: 0（正确）
```

### **5. 短路求值：谁先说了算？**

<code>||</code> 和 <code>??</code> 都使用了 **短路求值**，意思是如果左边的值能决定结果，右边的值就不会被计算。但两者的判断标准不同——<code>||</code> 会在遇到任意“假值”时短路，而 <code>??</code> 只会在 <code>null</code> 或 <code>undefined</code> 时短路。

### **6.总结：怎么选？**

* 如果你需要处理所有的“假值”（包括 <code>0</code>、<code>false</code>、<code>""</code>），就用 <code>||</code>。
* 如果你只关心 <code>null</code> 和 <code>undefined</code>，而 <code>0</code>、<code>false</code> 这些值也是有效输入，那就用 <code>??</code>。

## **结合 **<code>**||**</code>** 和 **<code>**??**</code>**：多重保险，让代码更健壮！**

有时候，你可能会遇到更复杂的场景，既要处理 <code>null</code> 和 <code>undefined</code>，又需要对其他“假值”如 <code>0</code>、<code>false</code>、<code>""</code> 做特殊处理。这种情况下，我们可以把 <code>||</code> 和 <code>??</code> 结合起来，打造一个更加全面的“多重保险”逻辑。

### **为什么要同时用 **<code>**||**</code>** 和 **<code>**??**</code>**？**

<code>??</code> 负责处理 <code>**null**</code> 和 **<code>undefined</code>**，而 <code>||</code> 可以处理 **所有“假值”**（如 <code>0</code>、<code>false</code>、空字符串等）。有些情况下，你可能希望 <code>null</code> 和 <code>undefined</code> 返回默认值，而对于其他“假值”则使用不同的逻辑处理。

来看个例子：

**代码语言：**javascript

**复制**

```javascript
let result = (value ?? defaultValue) || fallbackValue;
```

在这段代码中，我们做了两步处理：

1.<code>value ?? defaultValue</code>：首先，<code>??</code> 检查 <code>value</code> 是否为 <code>null</code> 或 <code>undefined</code>。如果是，那么返回 <code>defaultValue</code>；如果不是，则直接返回 <code>value</code>。2. <code>|| fallbackValue</code>：接下来，结果再经过 <code>||</code>，如果这个结果是“假值”（例如 <code>0</code>、<code>false</code>、空字符串等），就返回 <code>fallbackValue</code>。

##### **举个栗子\*\*\*\*🌰**

假设你在处理一个分数系统，<code>null</code> 或 <code>undefined</code> 的时候要用默认分数，但同时如果分数是 <code>0</code> 也不能直接忽略，需要特殊处理。我们可以这样写：

**代码语言：**javascript

**复制**

```javascript
function getFinalScore(score) {
  return (score ?? 50) || "分数无效";
}

console.log(getFinalScore(null));  // 输出: 50（null 被 `??` 处理）
console.log(getFinalScore(0));     // 输出: "分数无效"（0 被 `||` 处理）
console.log(getFinalScore(80));    // 输出: 80（有效分数）
```

* <code>**null**</code>** 的情况**：<code>score</code> 是 <code>null</code> 时，<code>??</code> 返回默认值 50，<code>||</code> 不会再触发。
* <code>**0**</code>** 的情况**：<code>score</code> 是 <code>0</code> 时，<code>??</code> 不起作用，<code>||</code> 将 <code>0</code> 视为“假值”，返回 "分数无效"。
* **有效分数的情况**：<code>score</code> 是 80 时，直接返回 80。

### **什么时候使用这种组合？**

这种组合特别适合需要处理复杂逻辑的场景，比如：

* <code>null</code> 或 <code>undefined</code> 的时候，返回一个默认值；
* 其他“假值”（如 <code>0</code>、<code>false</code>）也需要单独处理，避免被误认为无效。

## **结束**

在 JavaScript 开发中，<code>??</code> 和 <code>||</code> 绝对是处理默认值的利器，虽然它们看上去很像，但实际应用中却有明显区别。<code>||</code> 会把很多值当作“假值”，包括 <code>0</code>、<code>false</code>、空字符串等；而 <code>??</code> 只关心 <code>null</code> 和 <code>undefined</code>。因此，合理选择这两个运算符，能让你避免不必要的 Bug，尤其是在处理特殊值的时候。


