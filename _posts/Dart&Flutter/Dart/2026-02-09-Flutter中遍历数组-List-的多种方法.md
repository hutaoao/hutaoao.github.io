---
title: Flutter中遍历数组-List-的多种方法
date: 2026-02-09
description: Flutter 中遍历数组（List）的多种方法
tags: [Dart&Flutter, Dart, flutter]
categories: [Dart&Flutter, Dart]
---
# Flutter 中遍历数组（List）的多种方法

<font style="color:rgb(15, 17, 21);">在 Flutter 中遍历数组（List）有多种方法，下面我为你详细介绍各种遍历方式及其使用场景：</font>

## <font style="color:rgb(15, 17, 21);">1. for 循环 - 最基础的方式</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
List<String> fruits = ['apple', 'banana', 'orange'];

// 基本的 for 循环
for (int i = 0; i < fruits.length; i++) {
  print('Index: $i, Fruit: ${fruits[i]}');
}
```

## <font style="color:rgb(15, 17, 21);">2. for-in 循环 - 简洁的遍历</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
List<String> fruits = ['apple', 'banana', 'orange'];

// for-in 循环（不需要索引时使用）
for (String fruit in fruits) {
  print('Fruit: $fruit');
}
```

## <font style="color:rgb(15, 17, 21);">3. forEach 方法 - 函数式编程风格</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
List<String> fruits = ['apple', 'banana', 'orange'];

// forEach 方法
fruits.forEach((fruit) {
  print('Fruit: $fruit');
});

// 或者更简洁的写法
fruits.forEach((fruit) => print('Fruit: $fruit'));
```

## <font style="color:rgb(15, 17, 21);">4. map 方法 - 转换数组元素</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
List<String> fruits = ['apple', 'banana', 'orange'];

// map 方法（返回新的 Iterable）
List<String> capitalizedFruits = fruits.map((fruit) {
  return fruit.toUpperCase();
}).toList();

// 简洁写法
List<String> capitalizedFruits = fruits.map((fruit) => fruit.toUpperCase()).toList();

print(capitalizedFruits); // [APPLE, BANANA, ORANGE]
```

## <font style="color:rgb(15, 17, 21);">5. where 方法 - 过滤数组元素</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
List<int> numbers = [1, 2, 3, 4, 5, 6];

// 过滤偶数
List<int> evenNumbers = numbers.where((number) => number % 2 == 0).toList();
print(evenNumbers); // [2, 4, 6]
```

## <font style="color:rgb(15, 17, 21);">6. expand 方法 - 展开嵌套数组</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
List<List<int>> nestedList = [
  [1, 2],
  [3, 4],
  [5, 6]
];

// 展开为单层数组
List<int> flatList = nestedList.expand((list) => list).toList();
print(flatList); // [1, 2, 3, 4, 5, 6]
```

## <font style="color:rgb(15, 17, 21);">7. 使用索引的 forEach</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
List<String> fruits = ['apple', 'banana', 'orange'];

// 使用 asMap() 获取索引
fruits.asMap().forEach((index, fruit) {
  print('Index: $index, Fruit: $fruit');
});
```

## <font style="color:rgb(15, 17, 21);">8. 在 Flutter Widget 中的实际应用</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
class FruitList extends StatelessWidget {
  final List<String> fruits = ['apple', 'banana', 'orange'];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: ListView(
        children: [
          // 使用 map 生成 Widget 列表
          ...fruits.map((fruit) => ListTile(
            title: Text(fruit),
            trailing: Icon(Icons.arrow_forward),
          )).toList(),

          // 或者使用 Column + for-in
          Column(
            children: [
              for (String fruit in fruits) 
              ListTile(
                title: Text(fruit),
              ),
            ],
          ),
        ],
      ),
    );
  }
}
```

## <font style="color:rgb(15, 17, 21);">9. 其他有用的遍历方法</font>

<font style="color:rgb(15, 17, 21);">dart</font>

```dart
List<int> numbers = [1, 2, 3, 4, 5];

// reduce - 累积计算
int sum = numbers.reduce((value, element) => value + element);
print(sum); // 15

// fold - 带初始值的累积计算
int sumWithFold = numbers.fold(0, (previous, element) => previous + element);
print(sumWithFold); // 15

// any - 检查是否有元素满足条件
bool hasEven = numbers.any((number) => number % 2 == 0);
print(hasEven); // true

// every - 检查所有元素是否都满足条件
bool allPositive = numbers.every((number) => number > 0);
print(allPositive); // true
```

## <font style="color:rgb(15, 17, 21);">使用建议</font>

* **<font style="color:rgb(15, 17, 21);">基础遍历</font>**<font style="color:rgb(15, 17, 21);">：使用</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">for-in</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">或</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">forEach</font></code>
* **<font style="color:rgb(15, 17, 21);">需要索引</font>**<font style="color:rgb(15, 17, 21);">：使用</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">for</font></code><font style="color:rgb(15, 17, 21);"> </font><font style="color:rgb(15, 17, 21);">循环或</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">asMap().forEach()</font></code>
* **<font style="color:rgb(15, 17, 21);">转换数据</font>**<font style="color:rgb(15, 17, 21);">：使用</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">map</font></code>
* **<font style="color:rgb(15, 17, 21);">过滤数据</font>**<font style="color:rgb(15, 17, 21);">：使用</font><font style="color:rgb(15, 17, 21);"> </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">where</font></code>
* **<font style="color:rgb(15, 17, 21);">在 Widget 中</font>**<font style="color:rgb(15, 17, 21);">：推荐使用 </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">map</font></code><font style="color:rgb(15, 17, 21);"> 或 </font><code><font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">for-in</font></code><font style="color:rgb(15, 17, 21);"> 在列表中使用展开运算符</font>


