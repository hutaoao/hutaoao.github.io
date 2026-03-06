---
title: RN-Android端集成阿里云图形验证代码分析
date: 2025-06-04
description: RN-Android端集成阿里云图形验证代码分析
tags: [iOS, android]
categories: [iOS]
---
# RN-Android端集成阿里云图形验证代码分析

```java
package com.xiaota.xtbp;

import android.app.Activity;
import android.util.Log;

import com.alicom.gtcaptcha4.AlicomCaptcha4Client;
import com.alicom.gtcaptcha4.AlicomCaptcha4Config;
import com.facebook.react.bridge.Arguments;
import com.facebook.react.bridge.Promise;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;
import com.facebook.react.bridge.ReadableMap;
import com.facebook.react.bridge.WritableMap;
import com.facebook.react.modules.core.DeviceEventManagerModule;

import org.json.JSONObject;

import java.util.HashMap;
import java.util.Iterator;

public class AlicomCaptchaModule extends ReactContextBaseJavaModule {

    private final ReactApplicationContext reactContext;
    private AlicomCaptcha4Client captchaClient;
    private String verifyAPI;
    private boolean isInitialized = false;

    private static final String MODULE_NAME = "AlicomCaptchaModule";
    private static final String EVENT_VERIFY_RESULT = "onVerifyResult";
    private static final String TAG = "AlicomCaptcha";

    public AlicomCaptchaModule(ReactApplicationContext reactContext) {
        super(reactContext);
        this.reactContext = reactContext;
    }

    @Override
    public String getName() {
        return MODULE_NAME;
    }

    @ReactMethod
    public void init(ReadableMap config, Promise promise) {
        try {
            Activity currentActivity = getCurrentActivity();
            if (currentActivity == null) {
                promise.reject("NO_ACTIVITY", "无法获取当前Activity");
                return;
            }

            // 获取配置参数
            String captchaId = config.hasKey("captchaId") ?
                config.getString("captchaId") : "a5dcff3aea5e15493c72cef0c3ee6d37";

            if (config.hasKey("verifyAPI")) {
                verifyAPI = config.getString("verifyAPI");
            }

            currentActivity.runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    try {
                        // 创建配置参数
                        HashMap<String, Object> params = new HashMap<>();
                        params.put("loading", "");
                        params.put("displayMode", -1);
                        params.put("bgColor", "#FFFFFFFF");
                        params.put("protocol", "https://");
                        params.put("useLocalOffline", false);

                        // 构建配置
                        AlicomCaptcha4Config.Builder builder = new AlicomCaptcha4Config.Builder();
//                                 .setDebug(false)
//                                 .setResourcePath("https://static.geetest.com/v4/gt4-index.html")
//                                 .setLanguage("zh")
//                                 .setTimeOut(10000)
//                                 .setCanceledOnTouchOutside(true)
//                                 .setParams(params);

                        // 初始化客户端
                        captchaClient = AlicomCaptcha4Client.getClient(currentActivity);
                        captchaClient.setLogEnable(false);
                        captchaClient.init(captchaId, builder.build());

                        isInitialized = true;
                        Log.d(TAG, "阿里云验证SDK初始化成功");
                        promise.resolve(true);

                    } catch (Exception e) {
                        Log.e(TAG, "初始化失败", e);
                        promise.reject("INIT_ERROR", "初始化失败: " + e.getMessage());
                    }
                }
            });

        } catch (Exception e) {
            Log.e(TAG, "初始化异常", e);
            promise.reject("INIT_ERROR", "初始化异常: " + e.getMessage());
        }
    }

    @ReactMethod
    public void startVerify() {
        if (!isInitialized) {
            sendErrorResult("NOT_INITIALIZED", "请先调用init方法初始化SDK");
            return;
        }

        Activity currentActivity = getCurrentActivity();
        if (currentActivity == null) {
            sendErrorResult("NO_ACTIVITY", "无法获取当前Activity");
            return;
        }

        currentActivity.runOnUiThread(new Runnable() {
            @Override
            public void run() {
                try {
                    // 清除之前的监听器，避免重复添加
                    captchaClient.destroy();

                    // 重新初始化客户端（简单处理，避免监听器冲突）
                    AlicomCaptcha4Config.Builder builder = new AlicomCaptcha4Config.Builder();
//                             .setDebug(false)
//                             .setResourcePath("https://static.geetest.com/v4/gt4-index.html")
//                             .setLanguage("zh")
//                             .setTimeOut(10000)
//                             .setCanceledOnTouchOutside(true);

                    captchaClient.init("a5dcff3aea5e15493c72cef0c3ee6d37", builder.build());

                    // 设置新的监听器
                    captchaClient.addOnSuccessListener(new AlicomCaptcha4Client.OnSuccessListener() {
                        @Override
                        public void onSuccess(boolean status, String response) {
                            handleSuccessResponse(status, response);
                        }
                    });

                    captchaClient.addOnFailureListener(new AlicomCaptcha4Client.OnFailureListener() {
                        @Override
                        public void onFailure(String error) {
                            handleFailureResponse(error);
                        }
                    });

                    // 开始验证
                    captchaClient.verifyWithCaptcha();
                    Log.d(TAG, "开始阿里云图形验证");

                } catch (Exception e) {
                    Log.e(TAG, "启动验证失败", e);
                    sendErrorResult("VERIFY_ERROR", "启动验证失败: " + e.getMessage());
                }
            }
        });
    }

    private void handleSuccessResponse(boolean status, String response) {
        try {
            WritableMap resultParams = Arguments.createMap();
            resultParams.putBoolean("success", status);
            resultParams.putString("code", status ? "1" : "0");
//             resultParams.putString("result", response);

            // 创建result对象，包含所有响应数据
            WritableMap resultData = Arguments.createMap();

            if (status && response != null && !response.isEmpty()) {
                try {
                    JSONObject responseJson = new JSONObject(response);

                    // 将response中的所有字段放入result对象
                    Iterator<String> keys = responseJson.keys();
                    while (keys.hasNext()) {
                        String key = keys.next();
                        Object value = responseJson.get(key);

                        if (value instanceof String) {
                            resultData.putString(key, (String) value);
                        } else if (value instanceof Boolean) {
                            resultData.putBoolean(key, (Boolean) value);
                        } else if (value instanceof Integer) {
                            resultData.putInt(key, (Integer) value);
                        } else if (value instanceof Double) {
                            resultData.putDouble(key, (Double) value);
                        } else if (value instanceof Long) {
                            resultData.putDouble(key, ((Long) value).doubleValue());
                        } else if (value instanceof Float) {
                            resultData.putDouble(key, ((Float) value).doubleValue());
                        } else {
                            resultData.putString(key, value.toString());
                        }
                    }

                } catch (Exception e) {
                    Log.e(TAG, "解析JSON响应失败", e);
                    // 如果解析失败，将原始响应放入result
                    resultData.putString("rawResponse", response);
                }
            }

            // 将result对象放入外层
            resultParams.putMap("result", resultData);


            if (status) {
                JSONObject responseJson = new JSONObject(response);
                if (responseJson.has("token")) {
                    resultParams.putString("verifyToken", responseJson.getString("token"));
                }
                resultParams.putString("message", "验证成功");
            } else {
                resultParams.putString("message", "验证失败");
            }

            sendEvent(EVENT_VERIFY_RESULT, resultParams);

        } catch (Exception e) {
            sendErrorResult("PARSE_ERROR", "解析验证结果失败");
        }
    }

    private void handleFailureResponse(String error) {
        WritableMap errorParams = Arguments.createMap();
        errorParams.putBoolean("success", false);
        errorParams.putString("code", "VERIFY_ERROR");
        errorParams.putString("message", error);

        sendEvent(EVENT_VERIFY_RESULT, errorParams);
    }

    @ReactMethod
    public void destroy() {
        if (captchaClient != null) {
            captchaClient.destroy();
            captchaClient = null;
        }
        isInitialized = false;
        Log.d(TAG, "销毁阿里云验证SDK资源");
    }

    private void sendErrorResult(String code, String message) {
        WritableMap errorParams = Arguments.createMap();
        errorParams.putBoolean("success", false);
        errorParams.putString("code", code);
        errorParams.putString("message", message);

        sendEvent(EVENT_VERIFY_RESULT, errorParams);
    }

    private void sendEvent(String eventName, WritableMap params) {
        reactContext
                .getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
                .emit(eventName, params);
    }
}
```

```java
package com.xiaota.xtbp;

import com.facebook.react.ReactPackage;
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.uimanager.ViewManager;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class AlicomCaptchaPackage implements ReactPackage {

    @Override
    public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
        return Collections.emptyList();
    }

    @Override
    public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {
        List<NativeModule> modules = new ArrayList<>();
        modules.add(new AlicomCaptchaModule(reactContext));
        return modules;
    }
}
```

## <font style="color:rgb(64, 64, 64);">一、基础语法与结构</font>

### <font style="color:rgb(64, 64, 64);">1. 包声明（Package Declaration）</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

<font style="color:rgb(73, 73, 73);background-color:rgb(250, 250, 250);">package com.xiaota.xtbp;</font>

* <font style="color:rgb(64, 64, 64);">定义类所在的包路径，必须是文件的第一行有效代码</font>

### <font style="color:rgb(64, 64, 64);">2. 导入语句（Import Statements）</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
import android.app.Activity;
import android.util.Log;
import com.facebook.react.bridge.*;
```

* <font style="color:rgb(64, 64, 64);">用于引入其他包中的类</font>
* <font style="color:rgb(64, 64, 64);">Android 系统包：</font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">android.*</font>**</code>
* <font style="color:rgb(64, 64, 64);">React Native 包：</font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">com.facebook.react.bridge.*</font>**</code>
* <font style="color:rgb(64, 64, 64);">第三方SDK包：</font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">com.alicom.gtcaptcha4.*</font>**</code>

### <font style="color:rgb(64, 64, 64);">3. 类定义（Class Definition）</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

<font style="color:rgb(73, 73, 73);background-color:rgb(250, 250, 250);">public class AlicomCaptchaModule extends ReactContextBaseJavaModule</font>

* <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">public</font>**</code><font style="color:rgb(64, 64, 64);">：访问修饰符，表示该类是公开的</font>
* <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">extends</font>**</code><font style="color:rgb(64, 64, 64);">：继承，表示该类继承自</font><font style="color:rgb(64, 64, 64);"> </font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">ReactContextBaseJavaModule</font>**</code>

### <font style="color:rgb(64, 64, 64);">4. 常量定义</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
private static final String MODULE_NAME = "AlicomCaptchaModule";
private static final String EVENT_VERIFY_RESULT = "onVerifyResult";
```

* <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">static final</font>**</code><font style="color:rgb(64, 64, 64);">：定义常量，值不可修改</font>
* <font style="color:rgb(64, 64, 64);">命名规范：全大写，下划线分隔</font>

### <font style="color:rgb(64, 64, 64);">5. 成员变量（Instance Variables）</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
private final ReactApplicationContext reactContext;
private AlicomCaptcha4Client captchaClient;
private String verifyAPI;
private boolean isInitialized = false;
```

* <font style="color:rgb(64, 64, 64);">类的属性，用于存储对象状态</font>
* <font style="color:rgb(64, 64, 64);">常用修饰符：</font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">private</font>**</code><font style="color:rgb(64, 64, 64);">,</font><font style="color:rgb(64, 64, 64);"> </font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">protected</font>**</code><font style="color:rgb(64, 64, 64);">,</font><font style="color:rgb(64, 64, 64);"> </font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">public</font>**</code>

***

## <font style="color:rgb(64, 64, 64);">二、React Native 交互相关</font>

### <font style="color:rgb(64, 64, 64);">1. 模块命名</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
@Override
public String getName() {
    return MODULE_NAME;
}
```

* <font style="color:rgb(64, 64, 64);">必须重写的方法，返回模块名（JS端调用的名称）</font>

### <font style="color:rgb(64, 64, 64);">2. 导出方法给 JS 调用</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
@ReactMethod
public void init(ReadableMap config, Promise promise) {
    // 方法实现
}
```

* <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">@ReactMethod</font>**</code><font style="color:rgb(64, 64, 64);">：注解，表示该方法可被 JS 端调用</font>
* <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">ReadableMap</font>**</code><font style="color:rgb(64, 64, 64);">：从 JS 传递过来的对象（类似字典）</font>
* <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">Promise</font>**</code><font style="color:rgb(64, 64, 64);">：用于异步回调（resolve/reject）</font>

### <font style="color:rgb(64, 64, 64);">3. 向 JS 端发送事件</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
private void sendEvent(String eventName, WritableMap params) {
    reactContext
            .getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
            .emit(eventName, params);
}
```

* <font style="color:rgb(64, 64, 64);">使用</font><font style="color:rgb(64, 64, 64);"> </font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">RCTDeviceEventEmitter</font>**</code><font style="color:rgb(64, 64, 64);"> </font><font style="color:rgb(64, 64, 64);">发送事件到 JS</font>
* <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">WritableMap</font>**</code><font style="color:rgb(64, 64, 64);">：可写入的Map，用于构造要传递的数据</font>

### <font style="color:rgb(64, 64, 64);">4. 数据类型转换</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
// JS对象 → Java
ReadableMap config;
String captchaId = config.getString("captchaId");

// Java → JS对象
WritableMap resultParams = Arguments.createMap();
resultParams.putBoolean("success", true);
resultParams.putString("message", "成功");
```

* <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">Arguments.createMap()</font>**</code><font style="color:rgb(64, 64, 64);">：创建空的WritableMap</font>
* <font style="color:rgb(64, 64, 64);">各种put方法：</font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">putString</font>**</code><font style="color:rgb(64, 64, 64);">,</font><font style="color:rgb(64, 64, 64);"> </font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">putBoolean</font>**</code><font style="color:rgb(64, 64, 64);">,</font><font style="color:rgb(64, 64, 64);"> </font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">putInt</font>**</code><font style="color:rgb(64, 64, 64);">,</font><font style="color:rgb(64, 64, 64);"> </font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">putDouble</font>**</code><font style="color:rgb(64, 64, 64);">,</font><font style="color:rgb(64, 64, 64);"> </font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">putMap</font>**</code>

***

## <font style="color:rgb(64, 64, 64);">三、Android 特有知识点</font>

### <font style="color:rgb(64, 64, 64);">1. Activity 相关</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
Activity currentActivity = getCurrentActivity();
if (currentActivity == null) {
    // 处理无Activity情况
    return;
}
```

* <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">getCurrentActivity()</font>**</code><font style="color:rgb(64, 64, 64);">：获取当前React Native所在的Activity</font>
* <font style="color:rgb(64, 64, 64);">所有UI操作必须在有Activity的情况下进行</font>

### <font style="color:rgb(64, 64, 64);">2. 主线程UI操作</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
currentActivity.runOnUiThread(new Runnable() {
    @Override
    public void run() {
        // 在这里执行UI操作
        captchaClient.verifyWithCaptcha();
    }
});
```

* <font style="color:rgb(64, 64, 64);">Android要求UI操作必须在主线程执行</font>
* <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">runOnUiThread()</font>**</code><font style="color:rgb(64, 64, 64);">：将代码抛到主线程执行</font>

### <font style="color:rgb(64, 64, 64);">3. 日志打印</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
Log.d(TAG, "调试信息");    // Debug
Log.i(TAG, "普通信息");    // Info  
Log.e(TAG, "错误信息");    // Error
```

* <font style="color:rgb(64, 64, 64);">用于调试和记录运行状态</font>
* <font style="color:rgb(64, 64, 64);">不同级别：VERBOSE, DEBUG, INFO, WARN, ERROR</font>

### <font style="color:rgb(64, 64, 64);">4. 异常处理</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
try {
    // 可能出错的代码
} catch (Exception e) {
    Log.e(TAG, "错误信息", e);
    promise.reject("ERROR_CODE", "错误描述");
}
```

* <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">try-catch</font>**</code><font style="color:rgb(64, 64, 64);">：捕获和处理异常</font>
* <font style="color:rgb(64, 64, 64);">良好的异常处理是Android开发的必备技能</font>

***

## <font style="color:rgb(64, 64, 64);">四、React Package 注册</font>

### <font style="color:rgb(64, 64, 64);">1. 实现 ReactPackage 接口</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

<font style="color:rgb(73, 73, 73);background-color:rgb(250, 250, 250);">public class AlicomCaptchaPackage implements ReactPackage</font>

* <font style="color:rgb(64, 64, 64);">必须实现</font><font style="color:rgb(64, 64, 64);"> </font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">ReactPackage</font>**</code><font style="color:rgb(64, 64, 64);"> </font><font style="color:rgb(64, 64, 64);">接口</font>

### <font style="color:rgb(64, 64, 64);">2. 注册Native模块</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
@Override
public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {
    List<NativeModule> modules = new ArrayList<>();
    modules.add(new AlicomCaptchaModule(reactContext));
    return modules;
}
```

* <font style="color:rgb(64, 64, 64);">在此处添加自定义的Native模块</font>
* <font style="color:rgb(64, 64, 64);">React Native启动时会自动扫描并注册这些模块</font>

### <font style="color:rgb(64, 64, 64);">3. 空视图管理器</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
@Override
public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
    return Collections.emptyList();
}
```

* <font style="color:rgb(64, 64, 64);">如果模块不包含自定义UI组件，返回空列表</font>

***

## <font style="color:rgb(64, 64, 64);">五、常用工具类</font>

### <font style="color:rgb(64, 64, 64);">1. JSON 处理</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
import org.json.JSONObject;
// 解析JSON字符串
JSONObject responseJson = new JSONObject(response);
String token = responseJson.getString("token");
```

### <font style="color:rgb(64, 64, 64);">2. 集合类</font>

<font style="color:rgb(82, 82, 82);background-color:rgb(250, 250, 250);">java</font>

```plain
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
// 创建列表
List<NativeModule> modules = new ArrayList<>();
```

***

## <font style="color:rgb(64, 64, 64);">六、代码结构总结</font>

<font style="color:rgb(64, 64, 64);">一个典型的React Native Android Native模块包含：</font>

1. **<font style="color:rgb(64, 64, 64);">模块类</font>**<font style="color:rgb(64, 64, 64);">：继承</font><font style="color:rgb(64, 64, 64);"> </font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">ReactContextBaseJavaModule</font>**</code>
   * <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">getName()</font>**</code><font style="color:rgb(64, 64, 64);"> </font><font style="color:rgb(64, 64, 64);">- 返回模块名</font>
   * <code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">@ReactMethod</font>**</code><font style="color:rgb(64, 64, 64);"> </font><font style="color:rgb(64, 64, 64);">- 导出给JS调用的方法</font>
   * <font style="color:rgb(64, 64, 64);">事件发送机制</font>
2. **<font style="color:rgb(64, 64, 64);">Package类</font>**<font style="color:rgb(64, 64, 64);">：实现</font><font style="color:rgb(64, 64, 64);"> </font><code>**<font style="color:rgb(64, 64, 64);background-color:rgb(236, 236, 236);">ReactPackage</font>**</code>
   * <font style="color:rgb(64, 64, 64);">注册模块到React Native</font>
3. **<font style="color:rgb(64, 64, 64);">Android特性</font>**<font style="color:rgb(64, 64, 64);">：</font>
   * <font style="color:rgb(64, 64, 64);">Activity生命周期感知</font>
   * <font style="color:rgb(64, 64, 64);">主线程UI操作</font>
   * <font style="color:rgb(64, 64, 64);">异常处理</font>
   * <font style="color:rgb(64, 64, 64);">日志记录</font>


