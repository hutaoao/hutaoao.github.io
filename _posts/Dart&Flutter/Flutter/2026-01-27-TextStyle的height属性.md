---
title: TextStyle的height属性
date: 2026-01-27
description: TextStyle 的 height 属性
tags: [Dart&Flutter, Flutter]
categories: [Dart&Flutter, Flutter]
---
# TextStyle 的 height 属性

<font style="color:rgb(15, 17, 21);">在 Flutter 中，</font><code><font style="color:rgb(15, 17, 21);">TextStyle</font></code><font style="color:rgb(15, 17, 21);"> 的 </font><code><font style="color:rgb(15, 17, 21);">height</font></code><font style="color:rgb(15, 17, 21);"> 属性用于控制文本的行高（line height），但它有一些特殊的行为需要注意。</font>

## <font style="color:rgb(15, 17, 21);">height 属性介绍</font>

**<font style="color:rgb(15, 17, 21);">定义：</font>**<font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);">height</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">是一个</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);">double</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">值，表示文本行高与字体大小的比例。</font>

**<font style="color:rgb(15, 17, 21);">计算公式：</font>**

text

<font style="color:rgb(15, 17, 21);">实际行高 = fontSize \* height</font>

**<font style="color:rgb(15, 17, 21);">重要特性：</font>**

* <code><font style="color:rgb(15, 17, 21);">height = 1.0</font></code><font style="color:rgb(15, 17, 21);">：行高等于字体大小</font>
* <code><font style="color:rgb(15, 17, 21);">height = 1.5</font></code><font style="color:rgb(15, 17, 21);">：行高是字体大小的1.5倍</font>
* <code><font style="color:rgb(15, 17, 21);">height = null</font></code><font style="color:rgb(15, 17, 21);">：使用默认的行高（通常由字体本身决定）（默认值）</font>

## <font style="color:rgb(15, 17, 21);">基本用法</font>

### <font style="color:rgb(15, 17, 21);">1. 1.0倍行高</font>

```dart
Text(
  'Hello World',
  style: TextStyle(
    fontSize: 16.0,
    height: 1.0, // 行高 = 16.0 * 1.0 = 16.0
  ),
)
```

### <font style="color:rgb(15, 17, 21);">2. 1.5倍行高</font>

```dart
Text(
  'Hello World',
  style: TextStyle(
    fontSize: 16.0,
    height: 1.5, // 行高 = 16.0 * 1.5 = 24.0
  ),
)
```

### <font style="color:rgb(15, 17, 21);">3. 紧凑行高</font>

```dart
Text(
  'Hello World',
  style: TextStyle(
    fontSize: 16.0,
    height: 0.8, // 行高 = 16.0 * 0.8 = 12.8
  ),
)
```

## <font style="color:rgb(15, 17, 21);">实际应用示例</font>

### <font style="color:rgb(15, 17, 21);">多行文本行间距控制</font>

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

### <font style="color:rgb(15, 17, 21);">标题和正文字体搭配</font>

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

### <font style="color:rgb(15, 17, 21);">列表项文本</font>

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

## <font style="color:rgb(15, 17, 21);">与 lineSpacing 的区别</font>

<font style="color:rgb(15, 17, 21);">需要注意的是，</font><code><font style="color:rgb(15, 17, 21);">height</font></code><font style="color:rgb(15, 17, 21);"> 不是直接设置行间距，而是设置整个行高：</font>

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

## <font style="color:rgb(15, 17, 21);">响应式设计中的使用</font>

```dart
Text(
  '响应式文本',
  style: TextStyle(
    fontSize: 16.sp, // 使用sp单位
    height: 1.5,     // height是比例值，不需要单位
  ),
)
```

## <font style="color:rgb(15, 17, 21);">注意事项</font>

### <font style="color:rgb(15, 17, 21);">1. 单行文本的垂直居中</font>

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

### <font style="color:rgb(15, 17, 21);">2. 与 fontWeight 的配合</font>

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

### <font style="color:rgb(15, 17, 21);">3. 多语言文本考虑</font>

```dart
Text(
  'Hello 你好 नमस्ते', // 混合语言文本
  style: TextStyle(
    fontSize: 16.0,
    height: 1.6, // 对于包含中文、阿拉伯文等字符的文本，可能需要更大的行高
  ),
)
```

## <font style="color:rgb(15, 17, 21);">推荐的最佳实践</font>

1. **<font style="color:rgb(15, 17, 21);">正文文本</font>**<font style="color:rgb(15, 17, 21);">：</font><code><font style="color:rgb(15, 17, 21);">height: 1.5 - 1.7</font></code>
2. **<font style="color:rgb(15, 17, 21);">标题文本</font>**<font style="color:rgb(15, 17, 21);">：</font><code><font style="color:rgb(15, 17, 21);">height: 1.2 - 1.4</font></code>
3. **<font style="color:rgb(15, 17, 21);">注释文本</font>**<font style="color:rgb(15, 17, 21);">：</font><code><font style="color:rgb(15, 17, 21);">height: 1.3 - 1.5</font></code>
4. **<font style="color:rgb(15, 17, 21);">列表项</font>**<font style="color:rgb(15, 17, 21);">：</font><code><font style="color:rgb(15, 17, 21);">height: 1.3 - 1.6</font></code>

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

<code><font style="color:rgb(15, 17, 21);">height</font></code><font style="color:rgb(15, 17, 21);"> 属性是控制文本垂直布局的重要工具，合理使用可以显著改善应用的视觉效果和用户体验。</font>


