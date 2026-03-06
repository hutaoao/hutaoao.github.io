---
title: ReactNavigation6-x
date: 2025-11-16
description: ReactNavigation 6.x
tags: [ReactNative, ReactNative0.71, react]
categories: [ReactNative, ReactNative0.71]
---
# ReactNavigation 6.x
**npm install @react-navigation/native react-native-screens react-native-safe-area-context @react-navigation/drawer� @react-navigation/native-stack� @react-navigation/bottom-tabs**

:::

<font style="color:rgb(28, 30, 33);">React Native 0.60 更高版本执行：</font>

:::success
**npx pod-install ios**

:::

重新打包，尽情玩耍吧！！！

> **<font style="color:#DF2A3F;">稍有不注意就有坑！！！</font>**
>
> 使用<font style="color:#DF2A3F;"> </font>**<font style="color:#DF2A3F;">@react-navigation/drawer�</font>** 抽屉组件时需要注意：（我当时是最后单独安装的 \*\*@react-navigation/drawer�，\*\*可能一开始一起安装不会出现问题）
>
> * 下载依赖  `npm install react-native-gesture-handler react-native-reanimated`
> * 在 `index.js` or `App.js` 引用 `import 'react-native-gesture-handler'`
> * 在 `babel.config.js` 添加 `plugins: ['react-native-reanimated/plugin']`
> * 最后 <code><font style="color:rgb(57, 58, 52);background-color:rgb(246, 248, 250);">npx pod-install ios</font></code>
>
> 过程中编译可能还会报错，可以尝试 `npm start --reset-cache`

<details class="lake-collapse"><summary id="u2bf75247"><span class="ne-text" style="font-size: 16px">具体参考下面截图</span></summary><p id="udcaf3813" class="ne-p"><img src="https://cdn.nlark.com/yuque/0/2023/png/256708/1683797203778-42e68bb3-a414-4dab-bf3e-2995b861dbbf.png" width="824" id="u89b5e636" class="ne-image" style="font-size: 16px"><img src="https://cdn.nlark.com/yuque/0/2023/png/256708/1683797271782-71528f37-7a1b-41f8-9b59-5b99fa5a3fce.png" width="1972" id="u9cb16874" class="ne-image" style="font-size: 16px"></p></details>
## 基础用法
### 路由跳转
1. `navigation.navigate('RouterName')�` 跳转到指定路由（当前路由不做跳转）
2. `navigation.push('RouterName')�` 跳转到指定路由（不限制路由）
3. `navigation.goBack()` 返回上一级
4. `navigation.popToTop()` 返回到第一屏

### 传参

// 1、
navigation.navigate('Details', {
  itemId: 86,
  otherParam: 'anything you want here',
})

// 2、
navigation.navigate({
  name: 'Home',
  params: {post: postText},
  merge: true,
});


const {navigation, route} = props;
const {params} = route;


navigation.setParams({
  query: 'someText',
});


### 配置头部

#### 配置title

// 1.
<Stack.Screen
  name="Home"
  component={HomeScreen}
{% raw %}
  options={{title: 'My home'}}
{% endraw %}
/>

// 2.
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={({route}) => ({title: route.params.name})}
/>


navigation.setOptions({title: 'Updated!'})


#### 调整头部样式

* <code><font style="color:rgb(28, 30, 33);">headerStyle</font></code><font style="color:rgb(28, 30, 33);">: </font>包装页眉的视图的样式对象
* <code><font style="color:rgb(28, 30, 33);">headerTintColor</font></code><font style="color:rgb(28, 30, 33);">: </font>后退按钮和标题都使用此属性作为颜色
* <code><font style="color:rgb(28, 30, 33);">headerTitleStyle</font></code><font style="color:rgb(28, 30, 33);">: </font>自定义标题的fontFamily、fontWeight和其他Text样式的属性

function StackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
{% raw %}
        options={{
          title: 'My home',
          headerStyle: {
            backgroundColor: '#f4511e',
          },
          headerTintColor: '#fff',
          headerTitleStyle: {
            fontWeight: 'bold',
          },
        }}
{% endraw %}
      />
    </Stack.Navigator>
  );
}


#### 共享常用配置

function StackScreen() {
  return (
    <Stack.Navigator
{% raw %}
      screenOptions={{
        headerStyle: {
          backgroundColor: '#f4511e',
        },
        headerTintColor: '#fff',
        headerTitleStyle: {
          fontWeight: 'bold',
        },
      }}
{% endraw %}
    >
      <Stack.Screen
        name="Home"
        component={HomeScreen}
{% raw %}
        options={{ title: 'My home' }}
{% endraw %}
      />
    </Stack.Navigator>
  );
}


#### 用自定义组件替换标题

function LogoTitle() {
  return (
    <Image
{% raw %}
      style={{ width: 50, height: 50 }}
{% endraw %}
      source={require('@expo/snack-static/react-native-logo.png')}
    />
  );
}

function StackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
{% raw %}
        options={{ headerTitle: (props) => <LogoTitle {...props} /> }}
{% endraw %}
      />
    </Stack.Navigator>
  );
}


### 头部按钮

#### 向页眉添加按钮

* <code>**headerShown**�</code>\*\* 隐藏头部\*\*
* `headerTitle`
* `headerRight`
* `headerLeft` （可用于覆盖返回按钮）

......

### 嵌套导航器


> 更新: 2023-05-12 11:02:33  
> 原文: <https://www.yuque.com/hutaoao/blog/ovmxuz9o2nro0wzp>