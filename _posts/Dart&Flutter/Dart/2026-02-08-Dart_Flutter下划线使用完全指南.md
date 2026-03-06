---
title: Dart_Flutter下划线使用完全指南
date: 2026-02-08
description: Dart/Flutter 下划线使用完全指南
tags: [Dart&Flutter, Dart, dart, flutter]
categories: [Dart&Flutter, Dart]
---
# Dart/Flutter 下划线使用完全指南

## <font style="color:rgb(15, 17, 21);">一、概述</font>

<font style="color:rgb(15, 17, 21);">在 Dart 语言中，下划线 (</font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">_</font></code><font style="color:rgb(15, 17, 21);">) 是实现</font>**<font style="color:rgb(15, 17, 21);">访问控制</font>**<font style="color:rgb(15, 17, 21);">和</font>**<font style="color:rgb(15, 17, 21);">封装性</font>**<font style="color:rgb(15, 17, 21);">的核心机制。本指南将全面介绍 Dart 和 Flutter 中下划线的各种用法、含义和最佳实践。</font>

## <font style="color:rgb(15, 17, 21);">二、基本规则</font>

### <font style="color:rgb(15, 17, 21);">1. 核心原则</font>

* <font style="color:rgb(15, 17, 21);">单下划线前缀：使标识符在当前库（文件）中</font>**<font style="color:rgb(15, 17, 21);">私有化</font>**
* <font style="color:rgb(15, 17, 21);">没有下划线：标识符是</font>**<font style="color:rgb(15, 17, 21);">公开的</font>**<font style="color:rgb(15, 17, 21);">，可以被其他库导入使用</font>
* <font style="color:rgb(15, 17, 21);">双下划线：</font>**<font style="color:rgb(15, 17, 21);">命名约定</font>**<font style="color:rgb(15, 17, 21);">，没有额外语言含义，但常用于表示更深层的内部实现</font>

### <font style="color:rgb(15, 17, 21);">2. 可见性范围</font>

| <font style="color:rgb(15, 17, 21);">位置</font> | <font style="color:rgb(15, 17, 21);">无下划线</font> | <font style="color:rgb(15, 17, 21);">单下划线</font> | <font style="color:rgb(15, 17, 21);">双下划线</font> |
| --- | --- | --- | --- |
| <font style="color:rgb(15, 17, 21);">类定义</font> | <font style="color:rgb(15, 17, 21);">跨文件可见</font> | <font style="color:rgb(15, 17, 21);">仅当前文件</font> | <font style="color:rgb(15, 17, 21);">仅当前文件（约定）</font> |
| <font style="color:rgb(15, 17, 21);">类成员</font> | <font style="color:rgb(15, 17, 21);">公开访问</font> | <font style="color:rgb(15, 17, 21);">仅当前类</font> | <font style="color:rgb(15, 17, 21);">仅当前类（约定）</font> |
| <font style="color:rgb(15, 17, 21);">顶层定义</font> | <font style="color:rgb(15, 17, 21);">公开导入</font> | <font style="color:rgb(15, 17, 21);">文件私有</font> | <font style="color:rgb(15, 17, 21);">文件私有（约定）</font> |

## <font style="color:rgb(15, 17, 21);">三、详细用法解析</font>

### <font style="color:rgb(15, 17, 21);">1. 类定义中的下划线</font>

#### <font style="color:rgb(15, 17, 21);">公开类（可被其他文件导入）</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
// 其他文件可以：import 'xxx.dart' 然后使用 MyClass
class MyClass {
  // ...
}
```

#### <font style="color:rgb(15, 17, 21);">私有类（文件内部使用）</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">2. 类成员中的下划线</font>

#### <font style="color:rgb(15, 17, 21);">公开成员</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

#### <font style="color:rgb(15, 17, 21);">私有成员</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

**<font style="color:rgb(15, 17, 21);">重要限制</font>**<font style="color:rgb(15, 17, 21);">：</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">3. 顶层定义中的下划线</font>

#### <font style="color:rgb(15, 17, 21);">公开顶层定义</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
// constants.dart
const String APP_NAME = 'MyApp';
const Color PRIMARY_COLOR = Colors.blue;

// 其他文件可以：import 'constants.dart';
```

#### <font style="color:rgb(15, 17, 21);">私有顶层定义</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

## <font style="color:rgb(15, 17, 21);">四、Flutter 特殊用法</font>

### <font style="color:rgb(15, 17, 21);">1. StatefulWidget 模式</font>

#### <font style="color:rgb(15, 17, 21);">标准写法（单下划线）</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

#### <font style="color:rgb(15, 17, 21);">双下划线写法（强调关联性）</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">2. 私有构造函数模式</font>

#### <font style="color:rgb(15, 17, 21);">单例模式</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

#### <font style="color:rgb(15, 17, 21);">Builder 模式</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">3. 混入（Mixin）中的下划线</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

## <font style="color:rgb(15, 17, 21);">五、双下划线的具体使用场景</font>

### <font style="color:rgb(15, 17, 21);">1. 表示层级关系</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">2. 避免命名冲突</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">3. 临时或实验性代码标记</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

## <font style="color:rgb(15, 17, 21);">六、最佳实践指南</font>

### <font style="color:rgb(15, 17, 21);">1. 默认使用私有（推荐）</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">2. 使用 Getter/Setter 控制访问</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">3. Widget 开发规范</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">4. 状态管理组件</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

## <font style="color:rgb(15, 17, 21);">七、常见错误与注意事项</font>

### <font style="color:rgb(15, 17, 21);">1. 错误用法</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">2. 测试中的处理</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

### <font style="color:rgb(15, 17, 21);">3. 序列化注意事项</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

## <font style="color:rgb(15, 17, 21);">八、团队规范建议</font>

### <font style="color:rgb(15, 17, 21);">1. 基础规范</font>

<font style="color:rgb(15, 17, 21);">text</font>

```dart
项目下划线使用规范：
  1. 所有类成员默认使用下划线（私有化）
  2. Widget 的状态类使用单下划线：_WidgetNameState
  3. 仅在需要公开访问时才移除下划线
  4. 避免使用双下划线，除非有明确理由
```

### <font style="color:rgb(15, 17, 21);">2. 代码审查要点</font>

* <font style="color:rgb(15, 17, 21);">✅</font><font style="color:rgb(15, 17, 21);"> 检查是否有不必要的公开成员</font>
* <font style="color:rgb(15, 17, 21);">✅</font><font style="color:rgb(15, 17, 21);"> 确保私有成员不会被外部误用</font>
* <font style="color:rgb(15, 17, 21);">✅</font><font style="color:rgb(15, 17, 21);"> Widget 的 State 类是否适当私有化</font>
* <font style="color:rgb(15, 17, 21);">✅</font><font style="color:rgb(15, 17, 21);"> 常量是否适当暴露（公开或私有）</font>

### <font style="color:rgb(15, 17, 21);">3. 文档注释规范</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

## <font style="color:rgb(15, 17, 21);">九、总结</font>

### <font style="color:rgb(15, 17, 21);">核心要点</font>

1. **<font style="color:rgb(15, 17, 21);">单下划线是语言特性</font>**<font style="color:rgb(15, 17, 21);">：实现真正的私有化</font>
2. **<font style="color:rgb(15, 17, 21);">双下划线是约定</font>**<font style="color:rgb(15, 17, 21);">：没有额外语言支持，但可传达设计意图</font>
3. **<font style="color:rgb(15, 17, 21);">默认私有原则</font>**<font style="color:rgb(15, 17, 21);">：除非需要公开，否则使用下划线</font>
4. **<font style="color:rgb(15, 17, 21);">Widget 模式</font>**<font style="color:rgb(15, 17, 21);">：State 类通常私有化，使用</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">_WidgetNameState</font></code>

### <font style="color:rgb(15, 17, 21);">选择建议</font>

* **<font style="color:rgb(15, 17, 21);">新项目</font>**<font style="color:rgb(15, 17, 21);">：坚持使用单下划线标准</font>
* **<font style="color:rgb(15, 17, 21);">现有项目</font>**<font style="color:rgb(15, 17, 21);">：遵循现有约定，保持一致性</font>
* **<font style="color:rgb(15, 17, 21);">团队协作</font>**<font style="color:rgb(15, 17, 21);">：制定明确规范并文档化</font>
* **<font style="color:rgb(15, 17, 21);">开源库</font>**<font style="color:rgb(15, 17, 21);">：仔细设计公开 API，内部实现充分封装</font>

### <font style="color:rgb(15, 17, 21);">最终建议代码模板</font>

<font style="color:rgb(15, 17, 21);">dart</font>

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

<font style="color:rgb(15, 17, 21);">通过合理使用下划线，可以创建出封装良好、易于维护、API 清晰的 Flutter 应用。记住：</font>**<font style="color:rgb(15, 17, 21);">好的封装是良好软件设计的基石</font>**<font style="color:rgb(15, 17, 21);">。</font>


