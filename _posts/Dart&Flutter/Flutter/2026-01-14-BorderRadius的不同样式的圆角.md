---
title: BorderRadius的不同样式的圆角
date: 2026-01-14
description: BorderRadius 的不同样式的圆角
tags: [Dart&Flutter, Flutter]
categories: [Dart&Flutter, Flutter]
---
# BorderRadius 的不同样式的圆角

<font style="color:rgb(15, 17, 21);">在 Flutter 中，</font><code><font style="color:rgb(15, 17, 21);">BorderRadius</font></code><font style="color:rgb(15, 17, 21);"> 的不同构造函数用于创建不同样式的圆角。以下是它们的区别：</font>

## <font style="color:rgb(15, 17, 21);">1.</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);">BorderRadius.zero</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 无圆角</font>

dart

```dart
BorderRadius.zero
// 等同于：BorderRadius.all(Radius.zero)
// 所有角都没有圆角，直角
```

**<font style="color:rgb(15, 17, 21);">效果：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">四个角都是直角</font><font style="color:rgb(15, 17, 21);">\ </font>**<font style="color:rgb(15, 17, 21);">使用场景：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">不需要圆角时</font>

## <font style="color:rgb(15, 17, 21);">2.</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);">BorderRadius.circular()</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 统一圆角</font>

dart

```dart
BorderRadius.circular(10.0)
// 等同于：BorderRadius.all(Radius.circular(10.0))
// 所有角都有相同的圆角半径
```

**<font style="color:rgb(15, 17, 21);">效果：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">四个角都是相同的圆角</font><font style="color:rgb(15, 17, 21);">\ </font>**<font style="color:rgb(15, 17, 21);">使用场景：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">需要统一圆角时</font>

## <font style="color:rgb(15, 17, 21);">3.</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);">BorderRadius.all()</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 统一圆角（更灵活）</font>

dart

```dart
BorderRadius.all(Radius.circular(10.0))  // 圆形圆角
BorderRadius.all(Radius.elliptical(20.0, 10.0))  // 椭圆圆角
```

**<font style="color:rgb(15, 17, 21);">效果：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">四个角都有相同的圆角（可以是圆形或椭圆形）</font><font style="color:rgb(15, 17, 21);">\ </font>**<font style="color:rgb(15, 17, 21);">使用场景：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">需要统一但更复杂的圆角时</font>

## <font style="color:rgb(15, 17, 21);">4.</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);">BorderRadius.only()</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 指定角圆角</font>

dart

```dart
BorderRadius.only(
  topLeft: Radius.circular(10.0),
  topRight: Radius.circular(20.0),
  bottomLeft: Radius.circular(15.0),
  bottomRight: Radius.zero,  // 直角
)
```

**<font style="color:rgb(15, 17, 21);">效果：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">可以单独设置每个角的圆角</font><font style="color:rgb(15, 17, 21);">\ </font>**<font style="color:rgb(15, 17, 21);">使用场景：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">需要不同角有不同圆角时</font>

## <font style="color:rgb(15, 17, 21);">5.</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);">BorderRadius.vertical()</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 垂直方向圆角</font>

dart

```dart
BorderRadius.vertical(
  top: Radius.circular(20.0),    // 顶部两个角
  bottom: Radius.circular(10.0), // 底部两个角
)
```

**<font style="color:rgb(15, 17, 21);">效果：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">顶部两个角相同，底部两个角相同</font><font style="color:rgb(15, 17, 21);">\ </font>**<font style="color:rgb(15, 17, 21);">使用场景：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">列表项、卡片等需要顶部和底部不同圆角时</font>

## <font style="color:rgb(15, 17, 21);">6.</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);">BorderRadius.horizontal()</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 水平方向圆角</font>

dart

```dart
BorderRadius.horizontal(
  left: Radius.circular(15.0),  // 左边两个角
  right: Radius.circular(5.0),  // 右边两个角
)
```

**<font style="color:rgb(15, 17, 21);">效果：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">左边两个角相同，右边两个角相同</font><font style="color:rgb(15, 17, 21);">\ </font>**<font style="color:rgb(15, 17, 21);">使用场景：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">侧边栏、菜单等需要左右不同圆角时</font>

## <font style="color:rgb(15, 17, 21);">7.</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);">BorderRadius.lerp()</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">- 插值动画</font>

dart

```dart
BorderRadius.lerp(start, end, t)
// 在两个BorderRadius之间进行插值，用于动画
```

**<font style="color:rgb(15, 17, 21);">效果：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">根据时间因子 t 在 start 和 end 之间平滑过渡</font><font style="color:rgb(15, 17, 21);">\ </font>**<font style="color:rgb(15, 17, 21);">使用场景：</font>**<font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">圆角动画效果</font>

## <font style="color:rgb(15, 17, 21);">示例对比</font>

dart

```dart
// 1. 无圆角
Container(
  decoration: BoxDecoration(
    borderRadius: BorderRadius.zero,
    color: Colors.blue,
  ),
)

  // 2. 统一圆角
  Container(
  decoration: BoxDecoration(
    borderRadius: BorderRadius.circular(16.0),
    color: Colors.red,
  ),
)

  // 3. 指定角圆角
  Container(
  decoration: BoxDecoration(
    borderRadius: BorderRadius.only(
      topLeft: Radius.circular(20.0),
      topRight: Radius.circular(20.0),
    ),
    color: Colors.green,
  ),
)

  // 4. 垂直方向圆角
  Container(
  decoration: BoxDecoration(
    borderRadius: BorderRadius.vertical(
      top: Radius.circular(25.0),
    ),
    color: Colors.orange,
  ),
)

  // 5. 椭圆圆角
  Container(
  decoration: BoxDecoration(
    borderRadius: BorderRadius.all(
      Radius.elliptical(30.0, 15.0), // 水平30，垂直15
    ),
    color: Colors.purple,
  ),
)
```

## <font style="color:rgb(15, 17, 21);">实际应用场景</font>

dart

```dart
// 按钮 - 统一圆角
ElevatedButton(
  style: ElevatedButton.styleFrom(
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(8.0),
    ),
  ),
)

  // 卡片 - 顶部圆角
  Card(
  shape: RoundedRectangleBorder(
    borderRadius: BorderRadius.vertical(
      top: Radius.circular(16.0),
    ),
  ),
)

  // 对话框 - 所有角圆角
  AlertDialog(
  shape: RoundedRectangleBorder(
    borderRadius: BorderRadius.circular(20.0),
  ),
)

  // 列表项 - 第一个和最后一个不同
  ListView.builder(
  itemBuilder: (context, index) {
    return Container(
      margin: EdgeInsets.symmetric(horizontal: 16.0),
      decoration: BoxDecoration(
        borderRadius: index == 0 
        ? BorderRadius.vertical(top: Radius.circular(12.0))
        : index == itemCount - 1
        ? BorderRadius.vertical(bottom: Radius.circular(12.0))
        : BorderRadius.zero,
      ),
    );
  },
)
```

## <font style="color:rgb(15, 17, 21);">总结</font>

* <code>**<font style="color:rgb(15, 17, 21);">zero</font>**</code><font style="color:rgb(15, 17, 21);">: 无圆角</font>
* <code>**<font style="color:rgb(15, 17, 21);">circular</font>**</code><font style="color:rgb(15, 17, 21);">: 统一圆形圆角</font>
* <code>**<font style="color:rgb(15, 17, 21);">all</font>**</code><font style="color:rgb(15, 17, 21);">: 统一圆角（支持椭圆）</font>
* <code>**<font style="color:rgb(15, 17, 21);">only</font>**</code><font style="color:rgb(15, 17, 21);">: 自定义每个角</font>
* <code>**<font style="color:rgb(15, 17, 21);">vertical</font>**</code><font style="color:rgb(15, 17, 21);">: 垂直方向控制</font>
* <code>**<font style="color:rgb(15, 17, 21);">horizontal</font>**</code><font style="color:rgb(15, 17, 21);">: 水平方向控制</font>
* <code>**<font style="color:rgb(15, 17, 21);">lerp</font>**</code><font style="color:rgb(15, 17, 21);">: 动画插值</font>


