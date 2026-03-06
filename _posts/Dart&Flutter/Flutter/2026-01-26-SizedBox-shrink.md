---
title: SizedBox-shrink
date: 2026-01-26
description: SizedBox.shrink()
tags: [Dart&Flutter, Flutter]
categories: [Dart&Flutter, Flutter]
---
# SizedBox.shrink()

<code>SizedBox.shrink()</code> 是 Flutter 中一个非常实用的组件，下面详细介绍它的作用和日常用法：

## 作用

<code>SizedBox.shrink()</code> 创建一个**尺寸为 0×0 的空盒子**，它：

* 不占用任何布局空间
* 不渲染任何内容
* 是最轻量级的空组件

## 日常用法

### 1. 条件渲染的空状态（最常用）

dart

```dart
Widget build(BuildContext context) {
  return isLoading 
    ? CircularProgressIndicator()
    : SizedBox.shrink();
}
```

### 2. 替代老的 <code>Container()</code>

dart

```dart
// 不推荐 - Container() 有额外的绘制开销
condition ? MyWidget() : Container()

// 推荐 - SizedBox.shrink() 更轻量
condition ? MyWidget() : SizedBox.shrink()
```

### 3. 作为默认值或占位符

dart

```dart
class MyWidget extends StatelessWidget {
  final Widget? child;

  const MyWidget({this.child});

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        child ?? SizedBox.shrink(), // 如果没有子组件就返回空
        // ... 其他组件
      ],
    );
  }
}
```

### 4. 在 Row/Column 中条件渲染子项

dart

```dart
Column(
  children: [
    if (showTitle) Text('标题'),
    shouldShowDivider ? Divider() : SizedBox.shrink(),
    Text('内容'),
  ],
)
```

### 5. 替代空的 Center 或 Padding

dart

```dart
// 不推荐
condition ? MyWidget() : Center()

// 推荐 - 如果不需要居中效果
condition ? MyWidget() : SizedBox.shrink()
```

## 与其他空组件的对比

| 组件 | 尺寸 | 渲染开销 | 适用场景 |
| --- | --- | --- | --- |
| <code>SizedBox.shrink()</code> | 0×0 | 最小 | **推荐** 条件渲染的空状态 |
| <code>Container()</code> | 父级约束 | 中等 | 需要装饰效果时 |
| <code>SizedBox()</code> | 0×0 | 小 | 同 shrink()，但代码更长 |
| <code>Offstage()</code> | 保持布局 | 中等 | 需要保留组件状态时 |
| <code>Visibility(visible: false)</code> | 保持布局 | 中等 | 需要动画或保留状态时 |

## 实际示例

dart

```dart
// 用户头像条件渲染
Widget buildAvatar() {
  return user?.avatarUrl != null 
    ? CircleAvatar(backgroundImage: NetworkImage(user!.avatarUrl!))
    : SizedBox.shrink();
}

// 错误消息条件显示
Widget buildErrorMessage() {
  return errorMessage != null
    ? Text(errorMessage!, style: TextStyle(color: Colors.red))
    : SizedBox.shrink();
}

// 列表空状态
Widget buildList() {
  return items.isEmpty
    ? const Text('暂无数据')
    : ListView.builder(
      itemCount: items.length,
      itemBuilder: (context, index) => ListTile(title: Text(items[index])),
    );
}
```

## 总结

**优先使用\*\*\*\* **<code>**SizedBox.shrink()**</code> 当你需要：

* 条件渲染的空状态
* 轻量级的占位符
* 避免不必要的布局计算

它的简洁性和高性能使其成为 Flutter 开发中最常用的空组件。


