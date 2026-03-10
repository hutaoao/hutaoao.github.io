---
title: Dart_Flutter下划线使用完全指南
date: 2026-02-08
description: Dart/Flutter 下划线使用完全指南
tags: [移动开发, flutter, dart]
categories: [移动开发, Flutter]
---
# Dart/Flutter 下划线使用完全指南

## 一、概述

在 Dart 语言中，下划线 (<code>_</code>) 是实现**访问控制**和**封装性**的核心机制。本指南将全面介绍 Dart 和 Flutter 中下划线的各种用法、含义和最佳实践。

## 二、基本规则

### 1. 核心原则

* 单下划线前缀：使标识符在当前库（文件）中**私有化**
* 没有下划线：标识符是**公开的**，可以被其他库导入使用
* 双下划线：**命名约定**，没有额外语言含义，但常用于表示更深层的内部实现

### 2. 可见性范围

| 位置 | 无下划线 | 单下划线 | 双下划线 |
| --- | --- | --- | --- |
| 类定义 | 跨文件可见 | 仅当前文件 | 仅当前文件（约定） |
| 类成员 | 公开访问 | 仅当前类 | 仅当前类（约定） |
| 顶层定义 | 公开导入 | 文件私有 | 文件私有（约定） |

## 三、详细用法解析

### 1. 类定义中的下划线

#### 公开类（可被其他文件导入）

```dart
// 其他文件可以：import 'xxx.dart' 然后使用 MyClass
class MyClass {
  // ...
}
```

#### 私有类（文件内部使用）

```dart
// 只能在当前 .dart 文件中使用
class _PrivateHelper {
  // 常用作工具类、辅助类、实现类
}

// 示例：文件内多个组件共享的基类
abstract class _BaseDialog {
  Widget buildHeader();
  Widget buildContent();
}

class InfoDialog extends StatelessWidget {
  // 可以使用 _BaseDialog
}
```

### 2. 类成员中的下划线

#### 公开成员

```dart
class Counter {
  // 公开变量（通常不推荐，应通过getter暴露）
  int count = 0;

  // 公开方法
  void increment() {
    _internalIncrement();
  }
}
```

#### 私有成员

```dart
class Counter {
  // 私有变量：只能在本类内部访问
  int _internalCount = 0;

  // 公开getter
  int get count => _internalCount;

  // 私有方法：内部实现细节
  void _updateCount(int newValue) {
    _internalCount = newValue;
    _notifyListeners();
  }

  // 私有方法
  void _notifyListeners() {
    // 通知逻辑
  }
}
```

**重要限制**：

```dart
// 同一文件中的不同类不能互相访问私有成员
class ClassA {
  int _secret = 1;
}

class ClassB {
  void test() {
    var a = ClassA();
    // print(a._secret); // ❌ 编译错误
  }
}
```

### 3. 顶层定义中的下划线

#### 公开顶层定义

```dart
// constants.dart
const String APP_NAME = 'MyApp';
const Color PRIMARY_COLOR = Colors.blue;

// 其他文件可以：import 'constants.dart';
```

#### 私有顶层定义

```dart
// utils.dart
// 只能在当前文件使用
int _calculateSum(int a, int b) => a + b;
String _formatDate(DateTime date) => '...';

// 可以导出的公开接口
int publicCalculate(int a, int b) {
  return _calculateSum(a, b); // 内部使用私有函数
}
```

## 四、Flutter 特殊用法

### 1. StatefulWidget 模式

#### 标准写法（单下划线）

```dart
class MyWidget extends StatefulWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  _MyWidgetState createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  // 私有状态变量
  int _counter = 0;

  // 私有方法
  void _increment() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Text('Count: $_counter');
  }
}
```

#### 双下划线写法（强调关联性）

```dart
class MapDialog extends StatefulWidget {
  @override
  State<MapDialog> createState() => __MapDialogState();
}

// 双下划线强调这是 MapDialog 的专属内部类
// 不会被误认为是其他用途的类
class __MapDialogState extends State<MapDialog> {
  // 实现细节...
}
```

### 2. 私有构造函数模式

#### 单例模式

```dart
class AppPreferences {
  static final AppPreferences _instance = AppPreferences._internal();

  // 私有构造函数
  AppPreferences._internal();

  // 公开的工厂构造函数
  factory AppPreferences() {
    return _instance;
  }
}
```

#### Builder 模式

```dart
class ApiClient {
  final String _baseUrl;
  final Map<String, String> _headers;

  // 私有主构造函数
  ApiClient._(this._baseUrl, this._headers);

  // 公开的工厂构造函数
  factory ApiClient.create({
    required String baseUrl,
    Map<String, String>? headers,
  }) {
    final defaultHeaders = {
      'Content-Type': 'application/json',
      ...?headers,
    };
    return ApiClient._(baseUrl, defaultHeaders);
  }
}
```

### 3. 混入（Mixin）中的下划线

```dart
// 公开的混入
mixin ValidationMixin {
  bool isValidEmail(String email) {
    // 验证逻辑
    return _validateFormat(email);
  }

  // 混入中的私有方法
  bool _validateFormat(String email) {
    return RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(email);
  }
}
```

## 五、双下划线的具体使用场景

### 1. 表示层级关系

```dart
class ComplexComponent {
  // 一级内部方法
  void _setup() {
    __initialize();
    __configure();
  }

  // 二级内部方法（更底层）
  void __initialize() {
    // 核心初始化逻辑
  }

  void __configure() {
    // 配置逻辑
  }
}
```

### 2. 避免命名冲突

```dart
// 文件内有多个相关类时
class MyDialog extends StatefulWidget {
  @override
  State<MyDialog> createState() => __MyDialogState();
}

// 辅助工具类
class _MyDialogHelper {
  static Size calculateSize(BuildContext context) {
    // 计算逻辑
  }
}

// 明确的状态类
class __MyDialogState extends State<MyDialog> {
  // 不会被误认为是 _MyDialogHelper
}
```

### 3. 临时或实验性代码标记

```dart
class ExperimentalFeature {
  void stableMethod() {
    // 稳定实现
  }

  // 标记为实验性内部实现，可能被移除或更改
  void __experimentalImplementation() {
    // 不稳定的实现
  }
}
```

## 六、最佳实践指南

### 1. 默认使用私有（推荐）

```dart
class UserRepository {
  // ✅ 推荐：默认私有，通过方法控制访问
  final ApiClient _api;
  final CacheService _cache;

  UserRepository(this._api, this._cache);

  // 公开接口
  Future<User> getUser(int id) async {
    return await _fetchUser(id);
  }

  // 私有实现
  Future<User> _fetchUser(int id) async {
    // 实现细节...
  }
}
```

### 2. 使用 Getter/Setter 控制访问

```dart
class Counter {
  // 私有变量
  int _value = 0;

  // 公开getter（只读）
  int get value => _value;

  // 控制修改
  void increment() {
    _value++;
    _onValueChanged();
  }

  void _onValueChanged() {
    // 内部处理
  }
}
```

### 3. Widget 开发规范

```dart
// ✅ 推荐：标准的 Flutter Widget 写法
class CustomButton extends StatelessWidget {
  // 公开属性
  final String label;
  final VoidCallback? onPressed;

  const CustomButton({
    required this.label,
    this.onPressed,
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return _buildButton(context);
  }

  // 私有构建方法
  Widget _buildButton(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      child: Text(label),
    );
  }
}
```

### 4. 状态管理组件

```dart
class AuthProvider extends ChangeNotifier {
  // 私有状态
  User? _user;
  AuthStatus _status = AuthStatus.initial;

  // 公开状态访问
  User? get user => _user;
  AuthStatus get status => _status;

  // 公开操作方法
  Future<void> login(String email, String password) async {
    _status = AuthStatus.loading;
    notifyListeners();

    try {
      _user = await _performLogin(email, password);
      _status = AuthStatus.authenticated;
    } catch (e) {
      _status = AuthStatus.error;
      _logError(e);
    }

    notifyListeners();
  }

  // 私有辅助方法
  Future<User> _performLogin(String email, String password) async {
    // 网络请求等实现
  }

  void _logError(Object error) {
    debugPrint('Login error: $error');
  }
}
```

## 七、常见错误与注意事项

### 1. 错误用法

```dart
// ❌ 错误：尝试跨类访问私有成员
class ClassA {
  int _private = 1;
}

class ClassB {
  void test() {
    var a = ClassA();
    // print(a._private); // 编译错误
  }
}

// ❌ 错误：尝试导入私有类
// other_file.dart
import 'my_file.dart';

void main() {
  // var obj = _PrivateClass(); // 编译错误
}
```

### 2. 测试中的处理

```dart
// 在测试中，如果需要访问私有成员进行测试
// 考虑以下几种方案：

// 方案1：通过公开接口测试
test('counter increments', () {
  final counter = Counter();
  counter.increment();
  expect(counter.value, 1); // 通过公开getter访问
});

// 方案2：使用测试专用子类（谨慎使用）
class TestableCounter extends Counter {
  // 可以添加测试辅助方法
  void testSetValue(int value) {
    // 注意：仍然无法直接访问 _value
    // 需要通过其他方式
  }
}
```

### 3. 序列化注意事项

```dart
class UserModel {
  final String name;
  final String _internalId; // 不会被序列化

  UserModel(this.name, this._internalId);

  Map<String, dynamic> toJson() {
    return {
      'name': name,
      // '_internalId': _internalId, // 通常不序列化私有字段
    };
  }
}
```

## 八、团队规范建议

### 1. 基础规范

text

```dart
项目下划线使用规范：
  1. 所有类成员默认使用下划线（私有化）
  2. Widget 的状态类使用单下划线：_WidgetNameState
  3. 仅在需要公开访问时才移除下划线
  4. 避免使用双下划线，除非有明确理由
```

### 2. 代码审查要点

* ✅ 检查是否有不必要的公开成员
* ✅ 确保私有成员不会被外部误用
* ✅ Widget 的 State 类是否适当私有化
* ✅ 常量是否适当暴露（公开或私有）

### 3. 文档注释规范

```dart
/// 用户数据模型
class UserModel {
  /// 用户ID（内部使用）
  /// 
  /// 注意：这是内部标识符，不应暴露给客户端
  final String _internalId;

  /// 用户名（公开）
  final String name;

  UserModel(this._internalId, this.name);
}
```

## 九、总结

### 核心要点

1. **单下划线是语言特性**：实现真正的私有化
2. **双下划线是约定**：没有额外语言支持，但可传达设计意图
3. **默认私有原则**：除非需要公开，否则使用下划线
4. **Widget 模式**：State 类通常私有化，使用 <code>_WidgetNameState</code>

### 选择建议

* **新项目**：坚持使用单下划线标准
* **现有项目**：遵循现有约定，保持一致性
* **团队协作**：制定明确规范并文档化
* **开源库**：仔细设计公开 API，内部实现充分封装

### 最终建议代码模板

```dart
// 文件：my_widget.dart
import 'package:flutter/material.dart';

/// 自定义组件
class MyWidget extends StatefulWidget {
  final String title;

  const MyWidget({
    required this.title,
    Key? key,
  }) : super(key: key);

  @override
  _MyWidgetState createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('${widget.title}: $_counter'),
        ElevatedButton(
          onPressed: _incrementCounter,
          child: const Text('Increment'),
        ),
      ],
    );
  }
}
```

通过合理使用下划线，可以创建出封装良好、易于维护、API 清晰的 Flutter 应用。记住：**好的封装是良好软件设计的基石**。


