---
title: TextStyle的height属性
date: 2026-01-27
description: TextStyle 的 height 属性
tags: [Dart&Flutter, Flutter]
categories: [Dart&Flutter, Flutter]
---
# TextStyle 的 height 属性

在 Flutter 中，<code>TextStyle</code> 的 <code>height</code> 属性用于控制文本的行高（line height），但它有一些特殊的行为需要注意。

## height 属性介绍

**定义：** <code>height</code> 是一个 <code>double</code> 值，表示文本行高与字体大小的比例。

**计算公式：**

text

实际行高 = fontSize \* height

**重要特性：**

* <code>height = 1.0</code>：行高等于字体大小
* <code>height = 1.5</code>：行高是字体大小的1.5倍
* <code>height = null</code>：使用默认的行高（通常由字体本身决定）（默认值）

## 基本用法

### 1. 1.0倍行高

```dart
Text(
  'Hello World',
  style: TextStyle(
    fontSize: 16.0,
    height: 1.0, // 行高 = 16.0 * 1.0 = 16.0
  ),
)
```

### 2. 1.5倍行高

```dart
Text(
  'Hello World',
  style: TextStyle(
    fontSize: 16.0,
    height: 1.5, // 行高 = 16.0 * 1.5 = 24.0
  ),
)
```

### 3. 紧凑行高

```dart
Text(
  'Hello World',
  style: TextStyle(
    fontSize: 16.0,
    height: 0.8, // 行高 = 16.0 * 0.8 = 12.8
  ),
)
```

## 实际应用示例

### 多行文本行间距控制

```dart
Text(
  '这是一个多行文本示例，用于演示height属性的效果。'
  '当文本内容较多时，合适的行高可以提高可读性。',
  style: TextStyle(
    fontSize: 14.0,
    height: 1.6, // 良好的阅读行高
    color: Colors.black87,
  ),
  maxLines: 3,
)
```

### 标题和正文字体搭配

```dart
Column(
  crossAxisAlignment: CrossAxisAlignment.start,
  children: [
    Text(
      '文章标题',
      style: TextStyle(
        fontSize: 24.0,
        height: 1.2, // 标题行高紧凑一些
        fontWeight: FontWeight.bold,
      ),
    ),
    SizedBox(height: 8.0),
    Text(
      '正文内容，这里是一段较长的文本描述。'
      '合适的行高可以让阅读体验更好。',
      style: TextStyle(
        fontSize: 16.0,
        height: 1.5, // 正文行高宽松一些
      ),
    ),
  ],
)
```

### 列表项文本

```dart
ListView.builder(
  itemCount: 5,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(
        '项目标题 $index',
        style: TextStyle(
          fontSize: 16.0,
          height: 1.3, // 列表项适中行高
          fontWeight: FontWeight.w500,
        ),
      ),
      subtitle: Text(
        '项目描述内容，这里是一些详细的说明文字。',
        style: TextStyle(
          fontSize: 14.0,
          height: 1.4, // 描述文字稍宽松
          color: Colors.grey[600],
        ),
      ),
    );
  },
)
```

## 与 lineSpacing 的区别

需要注意的是，<code>height</code> 不是直接设置行间距，而是设置整个行高：

```dart
Text(
  '第一行\n第二行\n第三行',
  style: TextStyle(
    fontSize: 16.0,
    height: 2.0, // 每行总高度 = 32.0
    // 实际行间距 = 行高 - 字体大小 = 16.0
  ),
)
```

## 响应式设计中的使用

```dart
Text(
  '响应式文本',
  style: TextStyle(
    fontSize: 16.sp, // 使用sp单位
    height: 1.5,     // height是比例值，不需要单位
  ),
)
```

## 注意事项

### 1. 单行文本的垂直居中

```dart
// 如果文本在容器中不垂直居中，可以调整height
Container(
  height: 50.0,
  color: Colors.grey[200],
  alignment: Alignment.center,
  child: Text(
    '居中文本',
    style: TextStyle(
      fontSize: 16.0,
      height: 1.0, // 尝试调整这个值来微调垂直位置
    ),
  ),
)
```

### 2. 与 fontWeight 的配合

```dart
Text(
  '加粗文本',
  style: TextStyle(
    fontSize: 16.0,
    height: 1.3,   // 加粗字体可能需要稍大的行高
    fontWeight: FontWeight.bold,
  ),
)
```

### 3. 多语言文本考虑

```dart
Text(
  'Hello 你好 नमस्ते', // 混合语言文本
  style: TextStyle(
    fontSize: 16.0,
    height: 1.6, // 对于包含中文、阿拉伯文等字符的文本，可能需要更大的行高
  ),
)
```

## 推荐的最佳实践

1. **正文文本**：<code>height: 1.5 - 1.7</code>
2. **标题文本**：<code>height: 1.2 - 1.4</code>
3. **注释文本**：<code>height: 1.3 - 1.5</code>
4. **列表项**：<code>height: 1.3 - 1.6</code>

```dart
// 最佳实践示例
Text(
  '这是一个设计良好的文本段落，'
  '使用了合适的行高来提升阅读体验。',
  style: TextStyle(
    fontSize: 15.0,
    height: 1.6, // 良好的行高比例
    color: Colors.black87,
  ),
)
```

<code>height</code> 属性是控制文本垂直布局的重要工具，合理使用可以显著改善应用的视觉效果和用户体验。


