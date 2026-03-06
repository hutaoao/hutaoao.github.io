---
title: SizedBox-shrink
date: 2026-01-26
description: SizedBox.shrink()
tags: [Dart&Flutter, Flutter]
categories: [Dart&Flutter, Flutter]
---
# SizedBox.shrink()

<code><font style="color:rgb(15, 17, 21);">SizedBox.shrink()</font></code><font style="color:rgb(15, 17, 21);"> 是 Flutter 中一个非常实用的组件，下面详细介绍它的作用和日常用法：</font>

## <font style="color:rgb(15, 17, 21);">作用</font>

<code><font style="color:rgb(15, 17, 21);">SizedBox.shrink()</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">创建一个</font>**<font style="color:rgb(15, 17, 21);">尺寸为 0×0 的空盒子</font>**<font style="color:rgb(15, 17, 21);">，它：</font>

* <font style="color:rgb(15, 17, 21);">不占用任何布局空间</font>
* <font style="color:rgb(15, 17, 21);">不渲染任何内容</font>
* <font style="color:rgb(15, 17, 21);">是最轻量级的空组件</font>

## <font style="color:rgb(15, 17, 21);">日常用法</font>

### <font style="color:rgb(15, 17, 21);">1. 条件渲染的空状态（最常用）</font>

dart

```dart
Widget build(BuildContext context) {
  return isLoading 
    ? CircularProgressIndicator()
    : SizedBox.shrink();
}
```

### <font style="color:rgb(15, 17, 21);">2. 替代老的</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);">Container()</font></code>

dart

```dart
// 不推荐 - Container() 有额外的绘制开销
condition ? MyWidget() : Container()

// 推荐 - SizedBox.shrink() 更轻量
condition ? MyWidget() : SizedBox.shrink()
```

### <font style="color:rgb(15, 17, 21);">3. 作为默认值或占位符</font>

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

### <font style="color:rgb(15, 17, 21);">4. 在 Row/Column 中条件渲染子项</font>

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

### <font style="color:rgb(15, 17, 21);">5. 替代空的 Center 或 Padding</font>

dart

```dart
// 不推荐
condition ? MyWidget() : Center()

// 推荐 - 如果不需要居中效果
condition ? MyWidget() : SizedBox.shrink()
```

## <font style="color:rgb(15, 17, 21);">与其他空组件的对比</font>

| <font style="color:rgb(15, 17, 21);">组件</font> | <font style="color:rgb(15, 17, 21);">尺寸</font> | <font style="color:rgb(15, 17, 21);">渲染开销</font> | <font style="color:rgb(15, 17, 21);">适用场景</font> |
| --- | --- | --- | --- |
| <code><font style="color:rgb(15, 17, 21);">SizedBox.shrink()</font></code> | <font style="color:rgb(15, 17, 21);">0×0</font> | <font style="color:rgb(15, 17, 21);">最小</font> | **<font style="color:rgb(15, 17, 21);">推荐</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">条件渲染的空状态</font> |
| <code><font style="color:rgb(15, 17, 21);">Container()</font></code> | <font style="color:rgb(15, 17, 21);">父级约束</font> | <font style="color:rgb(15, 17, 21);">中等</font> | <font style="color:rgb(15, 17, 21);">需要装饰效果时</font> |
| <code><font style="color:rgb(15, 17, 21);">SizedBox()</font></code> | <font style="color:rgb(15, 17, 21);">0×0</font> | <font style="color:rgb(15, 17, 21);">小</font> | <font style="color:rgb(15, 17, 21);">同 shrink()，但代码更长</font> |
| <code><font style="color:rgb(15, 17, 21);">Offstage()</font></code> | <font style="color:rgb(15, 17, 21);">保持布局</font> | <font style="color:rgb(15, 17, 21);">中等</font> | <font style="color:rgb(15, 17, 21);">需要保留组件状态时</font> |
| <code><font style="color:rgb(15, 17, 21);">Visibility(visible: false)</font></code> | <font style="color:rgb(15, 17, 21);">保持布局</font> | <font style="color:rgb(15, 17, 21);">中等</font> | <font style="color:rgb(15, 17, 21);">需要动画或保留状态时</font> |

## <font style="color:rgb(15, 17, 21);">实际示例</font>

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

## <font style="color:rgb(15, 17, 21);">总结</font>

**<font style="color:rgb(15, 17, 21);">优先使用</font>\*\*\*\*<font style="color:rgb(15, 17, 21);"> </font>**<code>**<font style="color:rgb(15, 17, 21);">SizedBox.shrink()</font>**</code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">当你需要：</font>

* <font style="color:rgb(15, 17, 21);">条件渲染的空状态</font>
* <font style="color:rgb(15, 17, 21);">轻量级的占位符</font>
* <font style="color:rgb(15, 17, 21);">避免不必要的布局计算</font>

<font style="color:rgb(15, 17, 21);">它的简洁性和高性能使其成为 Flutter 开发中最常用的空组件。</font>


> 更新: 2025-10-22 10:06:48  
> 原文: <https://www.yuque.com/hutaoao/blog/vip25tbmd6244ikv>