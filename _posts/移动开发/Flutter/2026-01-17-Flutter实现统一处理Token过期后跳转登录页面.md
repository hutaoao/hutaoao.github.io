---
title: Flutter实现统一处理Token过期后跳转登录页面
date: 2026-01-17
description: Flutter 实现统一处理Token过期后跳转登录页面
tags: [移动开发, Flutter, Flutter, flutter]
categories: [移动开发, Flutter]
---
# Flutter 实现统一处理Token过期后跳转登录页面

背景：

项目使用的是 `go_router` `dio`

实现token过期的时候拦截跳转到登录页

### 方式一：使用事件总线

#### 创建事件总线

```dart
class EventBus {
  //私有构造函数
  EventBus._internal();

  static EventBus? _instance;

  static EventBus get instance => _getInstance();

  static EventBus _getInstance() {
    return _instance ??= EventBus._internal();
  }

  // 存储事件回调方法
  final Map<String, Function> _events = {};

  // 设置事件监听
  void addListener(String eventKey, Function callback) {
    _events[eventKey] = callback;
  }

  // 移除监听
  void removeListener(String eventKey) {
    _events.remove(eventKey);
  }

  // 提交事件
  void commit(String eventKey) {
    _events[eventKey]?.call();
  }
}

class EventKeys {
  static const String logout = "Logout";
}
```

#### 监听登出事件

登录成功后，在主页面设置登出事件监听，事件响应后立即移除监听。

```dart
class TabPage extends StatefulWidget {
  const TabPage({Key? key}) : super(key: key);

  @override
  State<TabPage> createState() => _TabPageState();
}

class _TabPageState extends State<TabPage> {
  @override
  void initState() {
    super.initState();
    EventBus.instance.addListener(EventKeys.logout, () {
      // 移除事件监听
      EventBus.instance.removeListener(EventKeys.logout);
      // 跳转登录页面
      Navigator.of(context).pushNamedAndRemoveUntil('/login', (route) => false);
    });
  }

  @override
  void dispose() {
    // 移除事件监听
    EventBus.instance.removeListener(EventKeys.logout);
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return ...
  }
}
```

#### 网络请求拦截器中触发登出事件

```dart
class DioInterceptors extends Interceptor {
  /// 响应拦截
  @override
  void onResponse(Response response, ResponseInterceptorHandler handler) async {
    /// 这里你可以做
    // 请求成功是对数据做基本处理
    // 对某些单独的url返回数据做特殊处理
    // 根据公司的业务需求进行定制化处理
    var responseCode = response.data['code'];
    // { msg: 'token过期，请重新登录！', code: 10008 }
    // { msg: 'token无效，请重新登录！', code: 10009 }
    if (responseCode == 10000 || responseCode == 10009) {
      // 触发登出事件
      EventBus.instance.commit(EventKeys.logout);
    }

    // handler.next(response);
    super.onResponse(response, handler);
  }
}
```

### 方式二


