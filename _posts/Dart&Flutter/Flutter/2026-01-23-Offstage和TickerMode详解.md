---
title: Offstage和TickerMode详解
date: 2026-01-23
description: Offstage 和 TickerMode 详解
tags: [Dart&Flutter, Flutter]
categories: [Dart&Flutter, Flutter]
---
# Offstage 和 TickerMode 详解

## <font style="color:rgb(15, 17, 21);">Offstage</font>

### <font style="color:rgb(15, 17, 21);">什么是 Offstage？</font>

<code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">Offstage</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">是一个布局 widget，它可以控制子 widget 是否在视觉上显示，但</font>**<font style="color:rgb(15, 17, 21);">子 widget 仍然存在于 widget 树中</font>**<font style="color:rgb(15, 17, 21);">。</font>

### <font style="color:rgb(15, 17, 21);">核心属性</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
Offstage({
  Key? key,
  bool offstage = true,  // 关键属性：true 表示隐藏，false 表示显示
  Widget? child,
})
```

### <font style="color:rgb(15, 17, 21);">工作原理</font>

* <code>**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">offstage = true</font>**</code><font style="color:rgb(15, 17, 21);">：子 widget 不会被渲染到屏幕上，但仍然存在于 widget 树中，保持其状态</font>
* <code>**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">offstage = false</font>**</code><font style="color:rgb(15, 17, 21);">：子 widget 正常显示</font>

### <font style="color:rgb(15, 17, 21);">实际效果</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">在 Tab 切换中的应用</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

## <font style="color:rgb(15, 17, 21);">TickerMode</font>

### <font style="color:rgb(15, 17, 21);">什么是 TickerMode？</font>

<code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">TickerMode</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">控制子 widget 中的动画是否运行。它可以启用或禁用子 widget 树中的所有动画。</font>

### <font style="color:rgb(15, 17, 21);">核心属性</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
TickerMode({
  Key? key,
  required bool enabled,  // true: 动画运行，false: 动画暂停
  Widget? child,
})
```

### <font style="color:rgb(15, 17, 21);">工作原理</font>

* <code>**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">enabled = true</font>**</code><font style="color:rgb(15, 17, 21);">：子 widget 中的动画正常运行</font>
* <code>**<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">enabled = false</font>**</code><font style="color:rgb(15, 17, 21);">：子 widget 中的动画暂停，但动画状态被保持</font>

### <font style="color:rgb(15, 17, 21);">实际效果</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

## <font style="color:rgb(15, 17, 21);">Offstage + TickerMode 组合使用</font>

### <font style="color:rgb(15, 17, 21);">为什么需要组合？</font>

* **<font style="color:rgb(15, 17, 21);">Offstage 单独使用</font>**<font style="color:rgb(15, 17, 21);">：隐藏页面但动画仍在后台运行，消耗资源</font>
* **<font style="color:rgb(15, 17, 21);">TickerMode 单独使用</font>**<font style="color:rgb(15, 17, 21);">：暂停动画但页面仍然可见</font>
* **<font style="color:rgb(15, 17, 21);">组合使用</font>**<font style="color:rgb(15, 17, 21);">：既隐藏页面又暂停动画，完美优化</font>

### <font style="color:rgb(15, 17, 21);">典型用法</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
Offstage(
  offstage: !isVisible,  // 控制显示/隐藏
  child: TickerMode(
    enabled: isVisible,   // 控制动画运行/暂停
    child: YourPage(),    // 你的页面内容
  ),
)
```

## <font style="color:rgb(15, 17, 21);">在 Tab 应用中的完整示例</font>

### <font style="color:rgb(15, 17, 21);">优化后的 Tab 控制器</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

## <font style="color:rgb(15, 17, 21);">性能优势</font>

### <font style="color:rgb(15, 17, 21);">内存优化</font>

* **<font style="color:rgb(15, 17, 21);">未访问的页面</font>**<font style="color:rgb(15, 17, 21);">：不初始化，不占用内存</font>
* **<font style="color:rgb(15, 17, 21);">访问过的页面</font>**<font style="color:rgb(15, 17, 21);">：状态被保持，但动画暂停</font>

### <font style="color:rgb(15, 17, 21);">CPU 优化</font>

* **<font style="color:rgb(15, 17, 21);">隐藏的页面</font>**<font style="color:rgb(15, 17, 21);">：不参与渲染，节省 CPU</font>
* **<font style="color:rgb(15, 17, 21);">暂停的动画</font>**<font style="color:rgb(15, 17, 21);">：不消耗动画计算资源</font>

### <font style="color:rgb(15, 17, 21);">用户体验</font>

* **<font style="color:rgb(15, 17, 21);">快速切换</font>**<font style="color:rgb(15, 17, 21);">：已访问过的页面立即显示，无需重新加载</font>
* **<font style="color:rgb(15, 17, 21);">状态保持</font>**<font style="color:rgb(15, 17, 21);">：滚动位置、表单数据等都被保持</font>

## <font style="color:rgb(15, 17, 21);">注意事项</font>

1. **<font style="color:rgb(15, 17, 21);">内存使用</font>**<font style="color:rgb(15, 17, 21);">：虽然优化了，但多个页面的状态仍然占用内存</font>
2. **<font style="color:rgb(15, 17, 21);">初始化时机</font>**<font style="color:rgb(15, 17, 21);">：根据需要决定何时初始化页面（首次访问或预加载）</font>
3. **<font style="color:rgb(15, 17, 21);">复杂页面</font>**<font style="color:rgb(15, 17, 21);">：对于特别复杂的页面，考虑手动管理资源释放</font>

## <font style="color:rgb(15, 17, 21);">替代方案比较</font>

| <font style="color:rgb(15, 17, 21);">方案</font> | <font style="color:rgb(15, 17, 21);">状态保持</font> | <font style="color:rgb(15, 17, 21);">内存使用</font> | <font style="color:rgb(15, 17, 21);">CPU 使用</font> | <font style="color:rgb(15, 17, 21);">实现复杂度</font> |
| --- | --- | --- | --- | --- |
| <font style="color:rgb(15, 17, 21);">IndexedStack</font> | <font style="color:rgb(15, 17, 21);">✅</font><font style="color:rgb(15, 17, 21);"> 很好</font> | <font style="color:rgb(15, 17, 21);">⚠️</font><font style="color:rgb(15, 17, 21);"> 较高</font> | <font style="color:rgb(15, 17, 21);">⚠️</font><font style="color:rgb(15, 17, 21);"> 较高</font> | <font style="color:rgb(15, 17, 21);">简单</font> |
| <font style="color:rgb(15, 17, 21);">Offstage + TickerMode</font> | <font style="color:rgb(15, 17, 21);">✅</font><font style="color:rgb(15, 17, 21);"> 很好</font> | <font style="color:rgb(15, 17, 21);">✅</font><font style="color:rgb(15, 17, 21);"> 较低</font> | <font style="color:rgb(15, 17, 21);">✅</font><font style="color:rgb(15, 17, 21);"> 较低</font> | <font style="color:rgb(15, 17, 21);">中等</font> |
| <font style="color:rgb(15, 17, 21);">PageView</font> | <font style="color:rgb(15, 17, 21);">✅</font><font style="color:rgb(15, 17, 21);"> 很好</font> | <font style="color:rgb(15, 17, 21);">⚠️</font><font style="color:rgb(15, 17, 21);"> 中等</font> | <font style="color:rgb(15, 17, 21);">⚠️</font><font style="color:rgb(15, 17, 21);"> 中等</font> | <font style="color:rgb(15, 17, 21);">简单</font> |
| <font style="color:rgb(15, 17, 21);">手动销毁重建</font> | <font style="color:rgb(15, 17, 21);">❌</font><font style="color:rgb(15, 17, 21);"> 无</font> | <font style="color:rgb(15, 17, 21);">✅</font><font style="color:rgb(15, 17, 21);"> 最低</font> | <font style="color:rgb(15, 17, 21);">✅</font><font style="color:rgb(15, 17, 21);"> 最低</font> | <font style="color:rgb(15, 17, 21);">复杂</font> |

**<font style="color:rgb(15, 17, 21);">Offstage + TickerMode</font>**<font style="color:rgb(15, 17, 21);"> 在状态保持和性能之间提供了最佳平衡。</font>


