---
title: BorderRadius的不同样式的圆角
date: 2026-01-14
description: BorderRadius 的不同样式的圆角
tags: [移动开发, flutter]
categories: [移动开发, Flutter]
---
# BorderRadius 的不同样式的圆角

在 Flutter 中，<code>BorderRadius</code> 的不同构造函数用于创建不同样式的圆角。以下是它们的区别：

## 1. <code>BorderRadius.zero</code> - 无圆角

```dart
BorderRadius.zero
// 等同于：BorderRadius.all(Radius.zero)
// 所有角都没有圆角，直角
```

**效果：** 四个角都是直角\ **使用场景：** 不需要圆角时

## 2. <code>BorderRadius.circular()</code> - 统一圆角

```dart
BorderRadius.circular(10.0)
// 等同于：BorderRadius.all(Radius.circular(10.0))
// 所有角都有相同的圆角半径
```

**效果：** 四个角都是相同的圆角\ **使用场景：** 需要统一圆角时

## 3. <code>BorderRadius.all()</code> - 统一圆角（更灵活）

```dart
BorderRadius.all(Radius.circular(10.0))  // 圆形圆角
BorderRadius.all(Radius.elliptical(20.0, 10.0))  // 椭圆圆角
```

**效果：** 四个角都有相同的圆角（可以是圆形或椭圆形）\ **使用场景：** 需要统一但更复杂的圆角时

## 4. <code>BorderRadius.only()</code> - 指定角圆角

```dart
BorderRadius.only(
  topLeft: Radius.circular(10.0),
  topRight: Radius.circular(20.0),
  bottomLeft: Radius.circular(15.0),
  bottomRight: Radius.zero,  // 直角
)
```

**效果：** 可以单独设置每个角的圆角\ **使用场景：** 需要不同角有不同圆角时

## 5. <code>BorderRadius.vertical()</code> - 垂直方向圆角

```dart
BorderRadius.vertical(
  top: Radius.circular(20.0),    // 顶部两个角
  bottom: Radius.circular(10.0), // 底部两个角
)
```

**效果：** 顶部两个角相同，底部两个角相同\ **使用场景：** 列表项、卡片等需要顶部和底部不同圆角时

## 6. <code>BorderRadius.horizontal()</code> - 水平方向圆角

```dart
BorderRadius.horizontal(
  left: Radius.circular(15.0),  // 左边两个角
  right: Radius.circular(5.0),  // 右边两个角
)
```

**效果：** 左边两个角相同，右边两个角相同\ **使用场景：** 侧边栏、菜单等需要左右不同圆角时

## 7. <code>BorderRadius.lerp()</code> - 插值动画

```dart
BorderRadius.lerp(start, end, t)
// 在两个BorderRadius之间进行插值，用于动画
```

**效果：** 根据时间因子 t 在 start 和 end 之间平滑过渡\ **使用场景：** 圆角动画效果

## 示例对比

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

## 实际应用场景

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

## 总结

* <code>**zero**</code>: 无圆角
* <code>**circular**</code>: 统一圆形圆角
* <code>**all**</code>: 统一圆角（支持椭圆）
* <code>**only**</code>: 自定义每个角
* <code>**vertical**</code>: 垂直方向控制
* <code>**horizontal**</code>: 水平方向控制
* <code>**lerp**</code>: 动画插值


