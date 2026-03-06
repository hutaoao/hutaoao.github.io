---
title: 集合类型List_Set_Map
date: 2026-03-06
description: 集合类型List/Set/Map
tags: [Dart&Flutter, Dart]
categories: [Dart&Flutter, Dart]
---
# 集合类型List/Set/Map

## List

### 创建List方法

```dart
// 方法一
var list1 = <int>[1, 2, 3];

// 方法二
List list2 = <int>[1, 2, 3];

// 方法三 创建固定长度的List
List list3 = List<String>.filled(2, 'hello');

// 方法四 已被弃用
var list4 = new List();
```

### List 常用的属性和方法

#### 常用属性

* `length` 			长度
* `reversed`		翻转
* `isEmpty`		是否为空
* `isNotEmpty`		是否不为空

```dart
List list = [1, 2, 3, 4, 5];

print(list.length); // 5

print(list.isEmpty); // false

print(list.isNotEmpty); // true

print(list.reversed); // (5, 4, 3, 2, 1)

var newList = list.reversed.toList();
print(newList); // [5, 4, 3, 2, 1]
```

#### 常用方法

* `add()`		增加

```dart
List list = [1, 2, 3];
list.add(4); // 增加一个
print(list); // [1, 2, 3, 4]
```

* `addAll()`		拼接数组

```dart
List list = [1, 2, 3];
list.addAll([4， 5, 6]); // 拼接
print(list); // [1, 2, 3, 4, 5, 6]
```

* `indexOf()`	查找传入具体值 - 查找到返回索引值 否则返回 -1

```dart
List list = [1, 2, 3];
print(list.indexOf(2)); // 1
print(list.indexOf(10)); // -1
```

* `remove()`		删除

```dart
List list = ['张三', '李四', '王五'];
list.remove('李四');
print(list); // [张三, 王五]
```

* `removeAt(index)`	删除 - 根据索引删除

```dart
List list = ['张三', '李四', '王五'];
list.removeAt(1);
print(list); // [张三, 王五]
```

* `fillRange(start, end, value)`	修改

```dart
List list1 = ['张三', '李四', '王五'];
list1.fillRange(1, 2, 'aa');
print(list1); // [张三, aa, 王五]

List list2 = ['张三', '李四', '王五'];
list2.fillRange(1, 3, 'aa');
print(list2); // [张三, aa, aa]
```

* `insert(index, value)`	指定位置插入 - 一个

```dart
List list = ['张三', '李四', '王五'];
list.insert(1, 'aa');
print(list); // [张三, aa, 李四, 王五]
```

* `insertAll(index, list)`	指定位置插入List

```dart
List list = ['张三', '李四', '王五'];
list.insertAll(1, ['aa', 'bb']);
print(list); // [张三, aa, bb, 李四, 王五]
```

* `toList()`	其它类型转换成List
* `join()`		List转换成字符串

```dart
List list = ['张三', '李四', '王五'];
String str = list.join('-');
print(str); // 张三-李四-王五
```

* `split()`	字符串转化成List

```dart
String str = '张三-李四-王五';
List list = str.split('-');
print(list); // [张三, 李四, 王五]
```

* `forEach()`
* `map()`
* `where`
* `any`
* `every`

## Set

* 主要功能就是去除数组中重复内容
* Set 是没有顺序且不能重复的集合，所以不能通过索引去获取值

```dart
var s = new Set();
s.add('香蕉');
s.add('苹果');
s.add('苹果');
print(s); // {香蕉, 苹果}

// 转换为List
var list = s.toList();
print(list); // [香蕉, 苹果]
```

集合去重

```dart
List list = ['张三', '李四', '王五', '李四', '王五'];
var s = new Set();
s.addAll(list);
print(s.toList()); // [张三, 李四, 王五]
```

## Map

### 创建Map方法

```dart
// 方法一
var person = {
  'name': '张三',
  'age': 20
}

// 方法二
var person = new Map();
m['name'] = '张三';
m['age'] = 20;
```

### 常用属性和方法

#### 常用属性

* `keys`		获取所有的key值

```dart
var person = {
  'name': '张三',
  'age': 20
};

print(person.keys); // (name, age)
print(person.keys.toList()); // [name, age]
```

* `values`		获取所有的value值

```dart
var person = {
  'name': '张三',
  'age': 20
};

print(person.values); // (张三, 20)
print(person.values.toList()); // [张三, 20]
```

* `isEmpty`	是否为空

```dart
var person = {
  'name': '张三',
  'age': 20
};

print(person.isEmpty); // false
```

* `isNotEmpty`	是否不为空

```dart
var person = {
  'name': '张三',
  'age': 20
};

print(person.isNotEmpty); // true
```

#### 常用方法

* `remove(key)`		删除指定key的数据

```dart
var person = {
  'name': '张三',
  'age': 20,
  'sex': '男'
};
person.remove('sex');
print(person); // {name: 张三, age: 20}
```

* `addAll({...})`	合并映射 给映射内增加属性

```dart
var person = {
  'name': '张三',
  'age': 20
};
person.addAll({
  'sex': '男',
  'wrok': ['程序员', '快递员']
});
print(person); // {name: 张三, age: 20, sex: 男, wrok: [程序员, 快递员]}
```

* `containsValue`	查看映射内的值 返回true/false

```dart
var person = {
  'name': '张三',
  'age': 20,
  'sex': '男'
};
print(person.containsValue('张三')); // true
```

* `forEach`
* `map`
* `where`
* `any`
* `every`

## 通用方法

### forEach

```dart
// List
List list = ['张三', '李四', '王五'];
list.forEach((value) {
  print(value); // 张三 李四 王五
});
```

```dart
// Set
var s = new Set();
s.addAll([1, 2, 3]);
s.forEach((value) => print(value)); // 1 2 3
```

```dart
// Maps
var person = {
  'name': '张三',
  'age': 20,
  'sex': '男'
};

person.forEach((key, value) {
  print('$key --- $value');
});
// name --- 张三
// age --- 20
// sex --- 男
```

### map

```dart
List list = [1, 2, 3];
var newList = list.map((value) {
  return value * 2;
});
print(newList); // (2, 4, 6)
print(newList.toList()); // [2, 4, 6]
```

### where

返回满足条件的集合

```dart
List list = [1, 2, 3, 5, 7, 8, 9];
var newList = list.where((value) {
  return value > 5;
});
print(newList); // (7, 8, 9)
print(newList.toList()); // [7, 8, 9]
```

### any

只要有一个条件满足的 就返回 true， 否则返回 false

```dart
List list = [1, 2, 3, 5, 7, 8, 9];
var f = list.any((value) {
  return value > 5;
});
print(f); // true
```

### every

每一个都要满足条件才会返回 true，否则返回 false

```dart
List list = [1, 2, 3, 5, 7, 8, 9];
var f = list.every((value) {
  return value > 5;
});
print(f); // false
```


