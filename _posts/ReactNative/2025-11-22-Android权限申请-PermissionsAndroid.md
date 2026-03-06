---
title: Android权限申请-PermissionsAndroid
date: 2025-11-22
description: Android权限申请-PermissionsAndroid
tags: [ReactNative, android]
categories: [ReactNative]
---
# Android权限申请-PermissionsAndroid

### <font style="color:rgb(79, 79, 79);">RN权限申请</font>

<font style="color:rgb(77, 77, 77);">最基础的一步，先在android的AndroidManifest.xml文件中添加权限声明</font>\ <font style="color:rgb(77, 77, 77);">就像这样。</font>

![1692842640132-a57d5c3d-9d98-464b-963f-cd6adda0760d.png](/assets/img/posts/ReactNative/1692842640132-a57d5c3d-9d98-464b-963f-cd6adda0760d-380597.png)

<font style="color:rgb(77, 77, 77);">然后使用reactNative自带的权限管理API，使用比较简单。</font>

### <font style="color:rgb(79, 79, 79);">PermissionsAndroid和它的三个方法</font>

<font style="color:rgb(77, 77, 77);">这个API主要使用的就是三个方法，用于检查是否拥有权限的</font><code>**<font style="color:rgb(77, 77, 77);">check</font>**</code><font style="color:rgb(77, 77, 77);">方法，请求权限的</font><code>**<font style="color:rgb(77, 77, 77);">request</font>**</code><font style="color:rgb(77, 77, 77);">方法，请求多个权限的</font><code>**<font style="color:rgb(77, 77, 77);">requestMultiple</font>**</code><font style="color:rgb(77, 77, 77);">方法，</font>

* <font style="color:rgb(77, 77, 77);">check(permission):检查传入的权限是否经过授权，返回的是一个promise，解析为布尔值。</font>
* <font style="color:rgb(77, 77, 77);">request（permission,\[rationale]）:传入需要请求的权限，第二个参数rationale对象可以看官方文档，尾部链接，这是个可选项，</font>
* <font style="color:rgb(77, 77, 77);">requestMultiple（permission）： 传入一个包含多个权限的数组。</font>

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">请求权限返回的结果如下：</font>

* <font style="color:rgba(0, 0, 0, 0.75);">GRANTED: ‘granted’， 表示用户已授权</font>
* <font style="color:rgba(0, 0, 0, 0.75);">DENIED: ‘denied’， 表示用户已拒绝</font>
* <font style="color:rgba(0, 0, 0, 0.75);">NEVER\_ASK\_AGAIN: ‘never\_ask\_again’，表示用户已拒绝，且不愿被再次询问。</font>

<font style="color:rgba(0, 0, 0, 0.75);"></font>

### <font style="color:rgb(79, 79, 79);">从android 6.0以下的版本说起</font>

<font style="color:rgb(77, 77, 77);">据说 </font><code><font style="color:rgb(77, 77, 77);">6.0</font></code><font style="color:rgb(77, 77, 77);"> 以下版本，权限写在</font>**<font style="color:rgb(77, 77, 77);">AndroidManifest.xml</font>**<font style="color:rgb(77, 77, 77);">文件里就好，这个时候权限都可以直接请求到，这个时候的check始终返回</font>**<font style="color:rgb(77, 77, 77);">true</font>**<font style="color:rgb(77, 77, 77);">，并且request请求始终返回</font>**<font style="color:rgb(77, 77, 77);">PermissionsAndroid.RESULTS.GRANTED</font>**<font style="color:rgb(77, 77, 77);">，\ </font><font style="color:rgb(77, 77, 77);">写法和下面六点零以上版本一样</font>

### <font style="color:rgb(79, 79, 79);">android6.0以上版本</font>

##### <font style="color:rgb(77, 77, 77);">请求单个相机权限的标准案例：</font>

```javascript
async requestCameraPermissions() {
  try {
    const granted = await PermissionsAndroid.request(
      PermissionsAndroid.PERMISSIONS.CAMERA,
      {
        title: '申请相机权限',
        message: '不给权限不干活',
        buttonPositive: 'OK',
      },
    );
    if (granted === PermissionsAndroid.RESULTS.GRANTED) {
      infoToast('已获取相机权限');
    } else {
      errorToast('获取权限失败');
    }
  } catch (err) {
    console.log(err);
  }
}
```

<font style="color:rgb(77, 77, 77);">因为是请求权限是个异步过程，所以需要async和await关键字，然后判断granted是否等于PermissionsAndroid.RESULTS.GRANTED来判断是否请求成功，</font>\ <font style="color:rgb(77, 77, 77);">然后再我的小米上运行：</font>

![1692842844150-7fba0b62-ed90-46a7-82af-90cfa452f556.png](/assets/img/posts/ReactNative/1692842844150-7fba0b62-ed90-46a7-82af-90cfa452f556-693816.png)

<font style="color:rgb(77, 77, 77);">然后就获取成功了</font>

<font style="color:rgb(77, 77, 77);"></font>

##### <font style="color:rgb(77, 77, 77);">获取定位权限标准案例</font>

```javascript
const granted = await PermissionsAndroid.request(
  PermissionsAndroid.PERMISSIONS.ACCESS_FINE_LOCATION,
  {
    title: "获取定位权限",
    message: '允许“灰蚁军团”使用你的位置？',
    buttonPositive: "允许",
    buttonNegative: "不允许"
  }
)
// 允许
if (granted === PermissionsAndroid.RESULTS.GRANTED) {
  infoToast('已获取定位权限');
}
// 拒绝
else {
  errorToast('获取定位权限失败');
}
```

### <font style="color:rgb(79, 79, 79);">获取多个权限</font>

<font style="color:rgb(77, 77, 77);">和上文差不多，见下文：</font>

```javascript
async requestAllPermissions() {
  try {
    //申请多个权限，传入一个权限对象数组
    const granted = await PermissionsAndroid.requestMultiple([
      PermissionsAndroid.PERMISSIONS.PROCESS_OUTGOING_CALLS,
      PermissionsAndroid.PERMISSIONS.CALL_PHONE,
      PermissionsAndroid.PERMISSIONS.USE_SIP,
    ]);
    if (
      granted['android.permission.CALL_PHONE'] ===
      PermissionsAndroid.RESULTS.GRANTED
    ) {
      console.log('CALL_PHONE 权限已获取');
    } 
    if (
      granted['android.permission.PROCESS_OUTGOING_CALLS'] ===
      PermissionsAndroid.RESULTS.GRANTED
    ) {
      console.log('PROCESS_OUTGOING_CALLS 权限已获取');
    } 
    if (
      granted['android.permission.USE_SIP'] ===
      PermissionsAndroid.RESULTS.GRANTED
    ) {
      console.log('USE_SIP 权限已获取');
    }
  } catch (err) {
    console.log(err);
  }
}
```

效果如下

![1692842899980-059793ab-b575-457d-90a6-eed23f377bee.png](/assets/img/posts/ReactNative/1692842899980-059793ab-b575-457d-90a6-eed23f377bee-733716.png)

<font style="color:rgb(77, 77, 77);">可以看到，确实获取了多个权限。</font>\ <font style="color:rgb(77, 77, 77);">可以把请求权限的方法放在项目初次进入或者放在需要使用权限的地方</font>


