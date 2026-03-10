---
title: Offstage和TickerMode详解
date: 2026-01-23
description: Offstage 和 TickerMode 详解
tags: [移动开发, flutter]
categories: [移动开发, Flutter]
---
# Offstage 和 TickerMode 详解

## Offstage

### 什么是 Offstage？

<code>Offstage</code> 是一个布局 widget，它可以控制子 widget 是否在视觉上显示，但**子 widget 仍然存在于 widget 树中**。

### 核心属性

```dart
Offstage({
  Key? key,
  bool offstage = true,  // 关键属性：true 表示隐藏，false 表示显示
  Widget? child,
})
```

### 工作原理

* <code>**offstage = true**</code>：子 widget 不会被渲染到屏幕上，但仍然存在于 widget 树中，保持其状态
* <code>**offstage = false**</code>：子 widget 正常显示

### 实际效果

```dart
// 这个 Text 不会显示，但它的状态被保持
Offstage(
  offstage: true,
  child: Text('我是隐藏的内容'),
)

  // 这个 Text 正常显示
  Offstage(
  offstage: false,
  child: Text('我是可见的内容'),
)
```

### 在 Tab 切换中的应用

```dart
Stack(
  children: [
    Offstage(
      offstage: _currentIndex != 0,  // 不是当前页就隐藏
      child: HomePage(),  // 但 HomePage 的状态被保持
    ),
    Offstage(
      offstage: _currentIndex != 1,
      child: ProfilePage(),
    ),
  ],
)
```

## TickerMode

### 什么是 TickerMode？

<code>TickerMode</code> 控制子 widget 中的动画是否运行。它可以启用或禁用子 widget 树中的所有动画。

### 核心属性

```dart
TickerMode({
  Key? key,
  required bool enabled,  // true: 动画运行，false: 动画暂停
  Widget? child,
})
```

### 工作原理

* <code>**enabled = true**</code>：子 widget 中的动画正常运行
* <code>**enabled = false**</code>：子 widget 中的动画暂停，但动画状态被保持

### 实际效果

```dart
// 这个动画会运行
TickerMode(
  enabled: true,
  child: RotationTransition(
    turns: _controller,
    child: Icon(Icons.refresh),
  ),
)

  // 这个动画会暂停
  TickerMode(
  enabled: false,
  child: RotationTransition(
    turns: _controller,
    child: Icon(Icons.refresh),
  ),
)
```

## Offstage + TickerMode 组合使用

### 为什么需要组合？

* **Offstage 单独使用**：隐藏页面但动画仍在后台运行，消耗资源
* **TickerMode 单独使用**：暂停动画但页面仍然可见
* **组合使用**：既隐藏页面又暂停动画，完美优化

### 典型用法

```dart
Offstage(
  offstage: !isVisible,  // 控制显示/隐藏
  child: TickerMode(
    enabled: isVisible,   // 控制动画运行/暂停
    child: YourPage(),    // 你的页面内容
  ),
)
```

## 在 Tab 应用中的完整示例

### 优化后的 Tab 控制器

```dart
class TabController extends StatefulWidget {
  @override
  _TabControllerState createState() => _TabControllerState();
}

class _TabControllerState extends State<TabController> {
  int _currentIndex = 0;
  final List<bool> _pageInitialized = [false, false, false, false];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Stack(
        children: [
          // 页面 0
          _buildPage(0, HomePage()),
          // 页面 1
          _buildPage(1, ProfilePage()),
          // 页面 2
          _buildPage(2, SettingsPage()),
          // 页面 3
          _buildPage(3, MessagesPage()),
        ],
      ),
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _currentIndex,
        onTap: (index) {
          setState(() {
            // 标记页面为已初始化
            if (!_pageInitialized[index]) {
              _pageInitialized[index] = true;
            }
            _currentIndex = index;
          });
        },
        items: [...],
      ),
    );
  }

  Widget _buildPage(int index, Widget page) {
    if (!_pageInitialized[index]) {
      return SizedBox.shrink(); // 未初始化的页面不渲染
    }

    return Offstage(
      offstage: _currentIndex != index, // 隐藏非当前页面
      child: TickerMode(
        enabled: _currentIndex == index, // 只允许当前页面运行动画
        child: page,
      ),
    );
  }
}
```

## 性能优势

### 内存优化

* **未访问的页面**：不初始化，不占用内存
* **访问过的页面**：状态被保持，但动画暂停

### CPU 优化

* **隐藏的页面**：不参与渲染，节省 CPU
* **暂停的动画**：不消耗动画计算资源

### 用户体验

* **快速切换**：已访问过的页面立即显示，无需重新加载
* **状态保持**：滚动位置、表单数据等都被保持

## 注意事项

1. **内存使用**：虽然优化了，但多个页面的状态仍然占用内存
2. **初始化时机**：根据需要决定何时初始化页面（首次访问或预加载）
3. **复杂页面**：对于特别复杂的页面，考虑手动管理资源释放

## 替代方案比较

| 方案 | 状态保持 | 内存使用 | CPU 使用 | 实现复杂度 |
| --- | --- | --- | --- | --- |
| IndexedStack | ✅ 很好 | ⚠️ 较高 | ⚠️ 较高 | 简单 |
| Offstage + TickerMode | ✅ 很好 | ✅ 较低 | ✅ 较低 | 中等 |
| PageView | ✅ 很好 | ⚠️ 中等 | ⚠️ 中等 | 简单 |
| 手动销毁重建 | ❌ 无 | ✅ 最低 | ✅ 最低 | 复杂 |

**Offstage + TickerMode** 在状态保持和性能之间提供了最佳平衡。


