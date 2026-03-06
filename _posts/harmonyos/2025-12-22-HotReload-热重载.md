---
title: HotReload-热重载
date: 2025-12-22
description: Hot Reload（热重载）
tags: [HarmonyOS]
categories: [HarmonyOS]
---
# Hot Reload（热重载）

> <font style="color:rgba(0, 0, 0, 0.9);">‼️</font><font style="color:rgba(0, 0, 0, 0.9);">这里的热重载限制很多，不像ReactNative、Flutter那样，切记注意，不然调试半天还以为是自己的BUG</font><font style="color:rgba(0, 0, 0, 0.9);">😂😂😂</font>



<font style="color:rgba(0, 0, 0, 0.9);">DevEco Studio提供Hot Reload（热重载）能力，支持开发者在真机或模拟器上运行/调试应用时，修改代码并保存后无需重启应用，在真机或模拟器上即可使用最新的代码，帮助开发者更快速地进行调试。</font>

**<font style="color:rgba(0, 0, 0, 0.9);">说明</font>**

<font style="color:rgba(0, 0, 0, 0.9);">Hot Reload支持Stage模型的ArkTS工程，暂不支持ArkTS卡片相关工程，不建议在hotReload模式下执行与ArkTS卡片的相关操作。</font>

## <font style="color:rgba(0, 0, 0, 0.9);">使用约束</font>
| **<font style="color:rgba(0, 0, 0, 0.9);">代码场景</font>** | **<font style="color:rgba(0, 0, 0, 0.9);">API 12</font>** | **<font style="color:rgba(0, 0, 0, 0.9);">限制条件</font>** |
| --- | --- | --- |
| <font style="color:rgba(0, 0, 0, 0.9);">UI代码修改</font> | <font style="color:rgba(0, 0, 0, 0.9);">支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">-</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">UI相应事件</font> | <font style="color:rgba(0, 0, 0, 0.9);">支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">-</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">多文件修改</font> | <font style="color:rgba(0, 0, 0, 0.9);">支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">-</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">类/接口/枚举</font> | <font style="color:rgba(0, 0, 0, 0.9);">部分支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">@Entry入口文件内class成员函数新增不支持；@Entry入口文件内枚举修改不支持。</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">匿名函数</font> | <font style="color:rgba(0, 0, 0, 0.9);">支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">-</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">Lambda函数</font> | <font style="color:rgba(0, 0, 0, 0.9);">支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">-</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">类继承</font> | <font style="color:rgba(0, 0, 0, 0.9);">部分支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">继承类和被继承类都不可以放在@Entry入口文件内。</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">成员方法和成员变量</font> | <font style="color:rgba(0, 0, 0, 0.9);">部分支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">@Entry入口文件struct Index内成员变量、成员函数不支持。</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">闭包函数</font> | <font style="color:rgba(0, 0, 0, 0.9);">支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">-</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">闭包变量</font> | <font style="color:rgba(0, 0, 0, 0.9);">部分支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">不支持@Entry闭包变量修改。</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">import和export</font> | <font style="color:rgba(0, 0, 0, 0.9);">部分支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">不支持import引入未加载的模块；只修改export default会导致热重载失效。</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">自定义组件</font> | <font style="color:rgba(0, 0, 0, 0.9);">支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">@Component自定义组件要放在非@Entry入口文件，使用import方式引入，且不能保留自定义组件热重载之前状态。</font> |
| <font style="color:rgba(0, 0, 0, 0.9);">代码文件（新增和删除）</font> | <font style="color:rgba(0, 0, 0, 0.9);">不支持</font> | <font style="color:rgba(0, 0, 0, 0.9);">-</font> |


<font style="color:rgba(0, 0, 0, 0.9);">对上面表格的解释说明：</font>

+ <font style="color:rgb(36, 39, 40);">当前API 版本为12，需要使用配套版本的设备镜像。</font>
+ <font style="color:rgb(36, 39, 40);">同一时间只支持一个工程的热重载，支持本工程一个模块（Module）内多个文件的修改。</font>
+ <font style="color:rgb(36, 39, 40);">支持修改UI代码，包括增删改，新增代码可以调用本代码文件或其他代码文件的类和方法。</font>
+ <font style="color:rgb(36, 39, 40);">支持调整组件响应事件的函数，修改函数实现，新增代码可以调用本代码文件或其它代码文件的类和方法。</font>
+ <font style="color:rgb(36, 39, 40);">支持ets声明式代码或ts代码文件的热重载，支持类、接口、枚举的增删改，支持匿名函数、Lambda函数的增删改。不支持@Entry入口文件内枚举的修改。</font>
+ <font style="color:rgb(36, 39, 40);">支持顶层闭包函数的增删改，支持顶层闭包变量的新增、删除，但不支持顶层闭包变量的修改。</font>
+ <font style="color:rgb(36, 39, 40);">支持@Component自定义组件的增删改，建议以import导入方式使用自定义组件，不支持@Component自定义组件在@Entry入口文件内的热重载。</font>
+ <font style="color:rgb(36, 39, 40);">支持class类成员函数和成员变量的增删改，但@Entry入口文件内的class成员函数新增不支持。支持class类继承，但继承类与被继承类都不可以放在@Entry入口文件内。</font>
+ <font style="color:rgb(36, 39, 40);">支持import引入文件的修改，并将修改添加到引入语句中使用，但要保证文件在热重载之前的状态已被引用。</font>
+ <font style="color:rgb(36, 39, 40);">仅支持运行单Ability工程的热重载，在多个Ability运行的工程中进行热重载可能导致异常。</font>
+ <font style="color:rgb(36, 39, 40);">Hot Reload不支持以下场景：</font>
    - <font style="color:rgb(36, 39, 40);">不支持增加和删除代码文件。</font>
    - <font style="color:rgb(36, 39, 40);">不支持import未加载模块的新增、修改。若一个代码文件在热重载启动工程的时候未被当前代码文件引用，则不支持在热重载过程中以新增import语句的方式在当前文件中引用此文件。若import语句在执行热重载之前是置灰的，则此文件在编译阶段不会参与编译，属于未加载模块，会导致热重载失败，此场景目前不能做到正确识别，目前会导致app闪退和弹出jsCrash。</font>
    - <font style="color:rgb(36, 39, 40);">不支持仅出现的export default这会导致引用的元素发生错乱，建议使用export default时和import修改一起使用。</font>
    - <font style="color:rgb(36, 39, 40);">在@Entry入口文件内不支持大部分装饰器的新增、修改。@Entry入口文件内不支持class类继承。不支持@Entry注释的struct Index内成员变量、成员函数的热重载。</font>
    - <font style="color:rgb(36, 39, 40);">在@Entry入口文件中不支持两个及以上的struct组件新增、修改，可以通过使用export、import导入导出实现struct组件的引用。</font>
    - <font style="color:rgb(36, 39, 40);">不支持资源修改热重载；不支持resource资源文件的修改（如修改string.json文件的内容），支持对资源引用的修改（如把$r('app.color.color')改成$r('app.color.color1')）；不支持so的热重载。</font>
    - <font style="color:rgb(36, 39, 40);">调试在命中断点时不支持热重载。</font>
    - <font style="color:rgb(36, 39, 40);">不支持主进程之外其他进程的模块的热重载。</font>
    - <font style="color:rgb(36, 39, 40);">不支持worker线程文件的热重载。</font>

## <font style="color:rgba(0, 0, 0, 0.9);">操作步骤</font>
1. <font style="color:rgb(36, 39, 40);">通过USB连接真机设备。</font>
2. <font style="color:rgb(36, 39, 40);">在下拉菜单中，将运行/调试配置切换为Hot Reload的配置</font>![1731467215618-86fde43a-aa8e-4f97-9b23-72186e928266.png](/assets/img/posts/harmonyos/1731467215618-86fde43a-aa8e-4f97-9b23-72186e928266-108488.png)<font style="color:rgb(36, 39, 40);">。</font>

![1731467274916-b1b1fb59-223c-4468-8f6a-ef1c9a0c9788.png](/assets/img/posts/harmonyos/1731467274916-b1b1fb59-223c-4468-8f6a-ef1c9a0c9788-902542.png)

3. <font style="color:rgb(36, 39, 40);">运行/调试应用，将代码编译打包运行/调试到真机上，请参考</font>[<font style="color:rgb(36, 39, 40);">使用本地真机运行应用/服务</font>](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-run-device-V5)<font style="color:rgb(36, 39, 40);">或</font>[<font style="color:rgb(36, 39, 40);">调试概述</font>](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-debug-device-V5)<font style="color:rgb(36, 39, 40);">。</font>
4. <font style="color:rgb(36, 39, 40);">修改代码后，可以通过如下操作，查看真机上修改后的显示效果。</font>
    - <font style="color:rgb(36, 39, 40);">点击Hot Reload</font>![1731467215680-d47b9174-f61d-425c-9968-21ac34510edf.png](/assets/img/posts/harmonyos/1731467215680-d47b9174-f61d-425c-9968-21ac34510edf-374316.png)<font style="color:rgb(36, 39, 40);">按钮：</font>

![1731467295814-e995e630-4d4c-4a01-98a3-cc3a101fb853.png](/assets/img/posts/harmonyos/1731467295814-e995e630-4d4c-4a01-98a3-cc3a101fb853-452022.png)

    - <font style="color:rgb(36, 39, 40);">通过快捷键方式触发Hot Reload：需要先在菜单栏点击</font>**<font style="color:rgb(36, 39, 40);">File > Settings</font>**<font style="color:rgb(36, 39, 40);">，选择</font>**<font style="color:rgb(36, 39, 40);">Tools > Actions on Save</font>**<font style="color:rgb(36, 39, 40);">，勾选</font>**<font style="color:rgb(36, 39, 40);">Perform hot reload</font>**<font style="color:rgb(36, 39, 40);">，点击</font>**<font style="color:rgb(36, 39, 40);">OK</font>**<font style="color:rgb(36, 39, 40);">完成设置。修改代码后通过快捷键</font>**<font style="color:rgb(36, 39, 40);">Ctrl + S</font>**<font style="color:rgb(36, 39, 40);">即可触发Hot Reload。</font>

![1731467348285-aee25a35-1d8d-46db-b8af-04fb96475187.png](/assets/img/posts/harmonyos/1731467348285-aee25a35-1d8d-46db-b8af-04fb96475187-502497.png)

5. <font style="color:rgb(36, 39, 40);">点击停止按钮终止运行/调试运行，退出Hot Reload模式。</font>



