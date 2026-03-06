---
title: firstWhere
date: 2026-02-06
description: firstWhere
tags: [Dart&Flutter, Dart, List常用方法]
categories: [Dart&Flutter, Dart, List常用方法]
---
# firstWhere

<code><font style="color:rgb(15, 17, 21);">firstWhere</font></code><font style="color:rgb(15, 17, 21);"> 是 Dart 中 List 的一个非常实用的方法，用于查找列表中满足条件的第一个元素。</font>

## <font style="color:rgb(15, 17, 21);">方法定义</font>

dart

```dart
E firstWhere(
  bool test(E element), 
  {E orElse()?}
)
```

## <font style="color:rgb(15, 17, 21);">参数说明</font>

* <code>**<font style="color:rgb(15, 17, 21);">test</font>**</code><font style="color:rgb(15, 17, 21);">：必需参数，一个返回 bool 的函数，用于测试每个元素</font>
* <code>**<font style="color:rgb(15, 17, 21);">orElse</font>**</code><font style="color:rgb(15, 17, 21);">：可选参数，当没有元素满足条件时调用的函数，返回默认值</font>

## <font style="color:rgb(15, 17, 21);">基本用法</font>

### <font style="color:rgb(15, 17, 21);">1. 基本查找</font>

dart

```dart
List<String> names = ['Alice', 'Bob', 'Charlie', 'David'];

// 查找第一个长度大于 3 的名字
String result = names.firstWhere((name) => name.length > 3);
print(result); // 输出: Alice

// 查找第一个以 'C' 开头的名字
String result2 = names.firstWhere((name) => name.startsWith('C'));
print(result2); // 输出: Charlie
```

### <font style="color:rgb(15, 17, 21);">2. 使用 orElse（推荐）</font>

dart

```dart
List<int> numbers = [1, 2, 3];

// 查找第一个大于 5 的数字，没有则返回 -1
int result = numbers.firstWhere(
  (number) => number > 5,
  orElse: () => -1
);
print(result); // 输出: -1
```

### <font style="color:rgb(15, 17, 21);">3. 不使用 orElse 的风险</font>

dart

```dart
List<int> numbers = [1, 2, 3];

// 这样会抛出 StateError
try {
  int result = numbers.firstWhere((number) => number > 5);
} catch (e) {
  print('错误: $e'); // 输出: Bad state: No element
}
```

## <font style="color:rgb(15, 17, 21);">在实际项目中的应用</font>

### <font style="color:rgb(15, 17, 21);">1. 您的用例</font>

dart

```dart
setState(() {
  currentOrgName = [
    userProvider.userInfo!.departmentName,
    userProvider.userInfo!.projectName,
  ].firstWhere((name) => name.isNotEmpty, orElse: () => '浙江省');
});
```

### <font style="color:rgb(15, 17, 21);">2. 用户角色权限查找</font>

dart

```dart
class User {
  final String name;
  final List<String> roles;

  User(this.name, this.roles);
}

List<User> users = [
  User('Alice', ['admin', 'user']),
  User('Bob', ['user']),
  User('Charlie', ['moderator', 'user']),
];

// 查找第一个有 admin 权限的用户
User admin = users.firstWhere(
  (user) => user.roles.contains('admin'),
  orElse: () => User('Default', ['user'])
);
```

### <font style="color:rgb(15, 17, 21);">3. 产品库存查找</font>

dart

```dart
class Product {
  final String name;
  final int stock;

  Product(this.name, this.stock);
}

List<Product> products = [
  Product('手机', 0),
  Product('电脑', 5),
  Product('平板', 3),
];

// 查找第一个有库存的产品
Product availableProduct = products.firstWhere(
  (product) => product.stock > 0,
  orElse: () => Product('暂无商品', 0)
);
```

## <font style="color:rgb(15, 17, 21);">高级用法</font>

### <font style="color:rgb(15, 17, 21);">1. 复杂条件查找</font>

dart

```dart
class Person {
  final String name;
  final int age;
  final String city;

  Person(this.name, this.age, this.city);
}

List<Person> people = [
  Person('Alice', 25, '北京'),
  Person('Bob', 30, '上海'),
  Person('Charlie', 28, '北京'),
];

// 查找北京的第一个年龄大于 26 的人
Person result = people.firstWhere(
  (person) => person.city == '北京' && person.age > 26,
  orElse: () => Person('未找到', 0, '')
);
```

### <font style="color:rgb(15, 17, 21);">2. 链式调用</font>

dart

```dart
List<String> data = ['', 'hello', '', 'world', ''];

// 多个条件组合
String result = data
  .where((item) => item.isNotEmpty)  // 先过滤空字符串
  .firstWhere(
  (item) => item.length > 4,       // 再找长度大于4的
  orElse: () => '默认值'
);
```

### <font style="color:rgb(15, 17, 21);">3. 与其他方法结合</font>

dart

```dart
List<Map<String, dynamic>> users = [
  {'name': 'Alice', 'active': true, 'score': 85},
  {'name': 'Bob', 'active': false, 'score': 92},
  {'name': 'Charlie', 'active': true, 'score': 78},
];

// 查找第一个活跃且分数大于80的用户
String userName = users
  .where((user) => user['active'] == true)
  .firstWhere(
  (user) => user['score'] > 80,
  orElse: () => {'name': '未找到'}
)['name'];
```

## <font style="color:rgb(15, 17, 21);">性能考虑</font>

<code><font style="color:rgb(15, 17, 21);">firstWhere</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">在找到第一个匹配元素后会立即返回，不会遍历整个列表，因此性能很好。</font>

dart

```dart
List<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// 只会检查到 3 就返回，不会检查后面的元素
int result = numbers.firstWhere((n) => n > 2);
print(result); // 输出: 3
```

## <font style="color:rgb(15, 17, 21);">错误处理最佳实践</font>

**<font style="color:rgb(15, 17, 21);">推荐总是使用</font>\*\*\*\*<font style="color:rgb(15, 17, 21);"> </font>**<code>**<font style="color:rgb(15, 17, 21);">orElse</font>**</code><font style="color:rgb(15, 17, 21);">：</font>

dart

```dart
// ✅ 推荐 - 安全
var result = list.firstWhere(test, orElse: () => defaultValue);

// ❌ 不推荐 - 可能抛出异常
var result = list.firstWhere(test);

// ✅ 或者使用 try-catch
try {
  var result = list.firstWhere(test);
} on StateError {
  // 处理没有找到元素的情况
}
```

## <font style="color:rgb(15, 17, 21);">与其他查找方法对比</font>

| <font style="color:rgb(15, 17, 21);">方法</font> | <font style="color:rgb(15, 17, 21);">返回值</font> | <font style="color:rgb(15, 17, 21);">找不到时</font> | <font style="color:rgb(15, 17, 21);">用途</font> |
| --- | --- | --- | --- |
| <code><font style="color:rgb(15, 17, 21);">firstWhere</font></code> | <font style="color:rgb(15, 17, 21);">第一个匹配元素</font> | <font style="color:rgb(15, 17, 21);">抛出异常或orElse</font> | <font style="color:rgb(15, 17, 21);">条件查找</font> |
| <code><font style="color:rgb(15, 17, 21);">first</font></code> | <font style="color:rgb(15, 17, 21);">第一个元素</font> | <font style="color:rgb(15, 17, 21);">抛出异常</font> | <font style="color:rgb(15, 17, 21);">获取第一个</font> |
| <code><font style="color:rgb(15, 17, 21);">where</font></code> | <font style="color:rgb(15, 17, 21);">所有匹配元素</font> | <font style="color:rgb(15, 17, 21);">空列表</font> | <font style="color:rgb(15, 17, 21);">过滤</font> |
| <code><font style="color:rgb(15, 17, 21);">singleWhere</font></code> | <font style="color:rgb(15, 17, 21);">唯一匹配元素</font> | <font style="color:rgb(15, 17, 21);">抛出异常</font> | <font style="color:rgb(15, 17, 21);">查找唯一元素</font> |


> 更新: 2025-10-21 10:05:08  
> 原文: <https://www.yuque.com/hutaoao/blog/emrpzlkr4nd41ddk>