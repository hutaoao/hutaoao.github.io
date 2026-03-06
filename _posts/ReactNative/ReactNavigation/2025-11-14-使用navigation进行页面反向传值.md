---
title: 使用navigation进行页面反向传值
date: 2025-11-14
description: 使用navigation进行页面反向传值
tags: [ReactNative, ReactNavigation]
categories: [ReactNative, ReactNavigation]
---
# 使用navigation进行页面反向传值

场景：A页面跳转到B页面进行一些新增/编辑操作，成功后返回到A页面进行刷新数据；但是A页面跳转到B页面查看详情时返回并不需要刷新数据。

> 此处的B页面通常可做 新增/编辑/详情 公共页面
>



下面这种做法行不通

```jsx
useEffect(() => {
  const focus = props.navigation.addListener('focus', () => {
    const {refresh} = props.route.params;
    if (refresh) {
      // do something
    }
  });
  return () => {
    focus();
  };
}, []);
```

```jsx
navigation.navigate('A', {refresh: true});
```



正确解决方案：

```jsx
// 父页面
function ParentScreen({ navigation, route }) {
  // 接收参数
  const { data } = route.params || {};
 
  // 更新参数的函数
  const updateData = (newData) => {
    if (newData && newData.refresh) {
      // do something
    }
  };
 
  // 导航到子页面
  const navigateToChild = () => {
    navigation.navigate('ChildScreen', { updateData });
  };
 
  return (
    <View>
      <Button title="Go to Child" onPress={navigateToChild} />
      <Text>Received Data: {data}</Text>
    </View>
  );
}
 
// 子页面
function ChildScreen({ navigation, route }) {
  const { updateData } = route.params || {};
 
  // 向父页面传递新值的例子
  const sendDataToParent = () => {
    updateData({refresh: true});
    navigation.goBack();
  };
 
  return (
    <View>
      <Button title="Send Data to Parent" onPress={sendDataToParent} />
    </View>
  );
}
```

此方案会有警告：

![1726797833196-a9c43173-4459-4b0c-a1e2-ad4f7c2cb176.png](/assets/img/posts/ReactNative/1726797833196-a9c43173-4459-4b0c-a1e2-ad4f7c2cb176-245678.png)

可以使用以下方法忽略警告

```jsx
import { LogBox } from 'react-native';

LogBox.ignoreLogs([
  'Non-serializable values were found in the navigation state',
]);
```



😮‍💨😮‍💨😮‍💨目前还没想到合适的解决方案



> 更新: 2026-03-06 11:40:14  
> 原文: <https://www.yuque.com/hutaoao/blog/mqypcgm952m9os18>