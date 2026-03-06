---
title: 探究react-native0-60-的autolink过程
date: 2025-12-12
description: 探究react-native 0.60+ 的autolink过程
tags: [ReactNative, react, native]
categories: [ReactNative]
---
# 探究react-native 0.60+ 的autolink过程
├─ lib
│    ├─ android
│    ├─ ios
│    └─ js
├─ docs
├─ node_modules
├─ index.js
```

android和ios的原生代码都被包裹在一个文件夹下

而规范的文件结构应该是这样的：

```plain
eact-native-xx库文件夹
├─ android
├─ ios
├─ docs
├─ node_modules
├─ index.js
```

为什么第一种文件结构无法autolink成功呢，这就要涉及到autolink的整个运行流程了，接下来就开始一步一步从源码理清这个过程吧（只探究android）

## 一、Link

众所周知，react-native link做的事情有3件：\
1、在`android/setting.gradle` 下添加:

```java
include ':react-native库'
project(':react-native库').projectDir = new File(rootProject.projectDir,     '../node_modules/react-native依赖/android')
```

2、在`android/app/build.gradle`

```java
dependencies {
+   implementation project(':react-native库')
}
```

3、在`android/app/src/main/java/com/your-app/MainApplication.java`

```java
+ import com.ocetnik.timer.BackgroundTimerPackage;

 @Override
 protected List<ReactPackage> getPackages() {
   return Arrays.<ReactPackage>asList(
      +   new xx依赖Package()
   );
 }
```

之后项目就能调用到此原生依赖

## 二、autolink

那么autolink是如何代替上面这三个配置呢，我们发现autolink时并不会在这3个文件上添加任何代码(其实，autolink发生在项目编译打包过程中，而不是安装依赖时)\
但是，这三处地方比以往多了一些变化\
1、在`setting.gradle`下新增了

```csharp
apply from: file("../node_modules/@react-native-community/cli-platform-android/native_modules.gradle"); applyNativeModulesSettingsGradle(settings)
include ':app'
```

2、在`android/app/build.gradle`下新增了

```csharp
apply from: file("../../node_modules/@react-native-community/cli-platform-android/native_modules.gradle");
 applyNativeModulesAppBuildGradle(project)
```

3、在`android/app/src/main/java/com/your-app/MainApplication.java`

```java
protected List<ReactPackage> getPackages() {
    @SuppressWarnings("UnnecessaryLocalVariable")
    List<ReactPackage> packages = new PackageList(this).getPackages();
    // Packages that cannot be autolinked yet can be added manually here, for example:
    // packages.add(new MyReactNativePackage());
    return packages;
}
```

就是这3处变化使得link过程自动化，同时避免了原生代码的改动\
接下来就一起来走进这3处地方代表的autolink的3个过程

### 1. 在setting.gradle里配置android依赖

`setting.gradle`的作用是配置项目所需要用到的依赖，gradle插件会加载里面的所有依赖并添加到android项目中

```csharp
// setting.gradle
apply from: file("../node_modules/@react-native-community/cli-platform-android/native_modules.gradle"); applyNativeModulesSettingsGradle(settings)
include ':app'
```

从代码可知，这条语句是去调用`/node_modules/@react-native-community/cli-platform-android/native_modules.gradle`这个gradle文件下的`applyNativeModulesSettingsGradle`方法\
我们直接去往这个文件

```dart
...
def autoModules = new ReactNativeModules(logger, projectRoot)

ext.applyNativeModulesSettingsGradle = { DefaultSettings defaultSettings, String root = null ->
  if (root != null) {
    logger.warn("${ReactNativeModules.LOG_PREFIX}Passing custom root is deprecated. CLI detects root automatically now.");
    logger.warn("${ReactNativeModules.LOG_PREFIX}Please remove second argument to `applyNativeModulesSettingsGradle`.");
  }
  autoModules.addReactNativeModuleProjects(defaultSettings) 
}
...
```

可以看到，`applyNativeModulesSettingsGradle`会去调用`ReactNativeModules`类的实例`autoModules`的`addReactNativeModuleProjects`方法\
我们来看下ReactNativeModlules这个类

```tsx
class ReactNativeModules {
  private Logger logger
  private String packageName
  private File root
  private ArrayList<HashMap<String, String>> reactNativeModules

  private static String LOG_PREFIX = ":ReactNative:"

  ReactNativeModules(Logger logger, File root) {
    this.logger = logger
    this.root = root

    def (nativeModules, packageName) = this.getReactNativeConfig()
    this.reactNativeModules = nativeModules
    this.packageName = packageName
  }

  /**
   * Include the react native modules android projects and specify their project directory
   */
  void addReactNativeModuleProjects(DefaultSettings defaultSettings) {
    reactNativeModules.forEach { reactNativeModule ->
      String nameCleansed = reactNativeModule["nameCleansed"]
      String androidSourceDir = reactNativeModule["androidSourceDir"]
      defaultSettings.include(":${nameCleansed}")
      defaultSettings.project(":${nameCleansed}").projectDir = new File("${androidSourceDir}")
    }
  }
  ...
}
```

可以看到`addReactNativeModuleProjects`做了一件事，就是把`reactNativeModules`数组的每一个项提取出来，取每个项的`nameCleansed`和`androidSourceDir`属性赋给`defaultSettings`

* 由构造函数可知，`reactNativeModules`是一个由`getReactNativeConfig`方法返回的数组，我们暂且不去管`getReactNativeConfig`的具体内容，只需要知道它返回了项目的包名`packageName`以及包含所有的依赖对象的数组`reactNativeModules`,每个对象包含了对应依赖的若干信息，包括路径，名称等等，`nameCleansed`是依赖的名称，`androidSouceDir`是依赖包的android路径。
* 那作为参数传过来的`defaultSettings`是什么呢?其实它是在`setting.gradle`里作为`applyNativeModulesSettingsGradle`的参数传进来，本质是`setting.gradle`文件的内置隐含对象`settings`

```python
... ;applyNativeModulesSettingsGradle(settings)
```

这里就需要额外普及一个小知识了

```php
include ":app"
// 完整写法是下面这个，省略了settings
settings.include(“:app”)
```

所以说，这两行代码

```dart
defaultSettings.include(":${nameCleansed}")
 defaultSettings.project(":${nameCleansed}").projectDir = new File("${androidSourceDir}")
```

就相当于在`setting.gradle`里添加

```dart
include ":${nameCleansed}"
project(":${nameCleansed}").projectDir = new File("${androidSourceDir}")
```

这样就成功实现了link的第一步

#### 2. 在build.gradle里配置依赖项并生成依赖包

```csharp
// app/bulid.gradle
apply from: file("../../node_modules/@react-native-community/cli-platform-android/native_modules.gradle");
 applyNativeModulesAppBuildGradle(project)
```

这条语句跟setting.gradle那里一样，目标文件都是`native_modules.gradle`，只不过这次调用的是其中的`applyNativeModulesAppBuildGradle`方法

```dart
ext.applyNativeModulesAppBuildGradle = { Project project, String root = null ->
  if (root != null) {
    logger.warn("${ReactNativeModules.LOG_PREFIX}Passing custom root is deprecated. CLI detects root automatically now");
    logger.warn("${ReactNativeModules.LOG_PREFIX}Please remove second argument to `applyNativeModulesAppBuildGradle`.");
  }
  autoModules.addReactNativeModuleDependencies(project)

  def generatedSrcDir = new File(buildDir, "generated/rncli/src/main/java")
  def generatedCodeDir = new File(generatedSrcDir, generatedFilePackage.replace('.', '/'))

  task generatePackageList {
    doLast {
      autoModules.generatePackagesFile(generatedCodeDir, generatedFileName, generatedFileContentsTemplate)
    }
  }

  preBuild.dependsOn generatePackageList // build之前执行生成包列表的task

  android {
    sourceSets {
      main {
        java {
          srcDirs += generatedSrcDir //添加一个java源文件目录
        }
      }
    }
  }
}
```

`applyNativeModulesAppBuildGradle`方法做了2件事：

* 1、调用`ReactNativeModules`类的实例`autoModules`的`addReactNativeModuleDependencies`方法,添加react-native依赖
* 2、创建一个生成一个package列表文件的task，并使其在构建之前执行，同时将文件路径配置到sourceSets（值得注意的是，这里的android指的是加载此gradle的project里的android，这里因为是app/bulid.gradle加载的此gradle，所以对应的是bulid.gradle的android，注意不要把android{}这种形式看成是配置，而应该看成是函数的调用）

##### 2.1 添加react-native依赖

看一下`addReactNativeModuleDependencies`方法

```cpp
void addReactNativeModuleDependencies(Project appProject) {
    reactNativeModules.forEach { reactNativeModule ->
      def nameCleansed = reactNativeModule["nameCleansed"]
      appProject.dependencies {
        // TODO(salakar): are other dependency scope methods such as `api` required?
        implementation project(path: ":${nameCleansed}")
      }
    }
  }
```

显而易见，这里相当于在`app/bulid.gralde`里的配置了各个react-native依赖,\
如同之前的：

```bash
dependencies {
    implementation project(':react-nativexx库')
    implementation project(':react-nativexxx库')
}
```

##### 2.2 生成Package列表文件

看一下`generatePackagesFile`方法

```dart
void generatePackagesFile(File outputDir, String generatedFileName, String generatedFileContentsTemplate) {
    ArrayList<HashMap<String, String>>[] packages = this.reactNativeModules
    String packageName = this.packageName

    String packageImports = ""
    String packageClassInstances = ""

    if (packages.size() > 0) {
      def interpolateDynamicValues = {
        it
                .replaceAll(~/([^.\w])(BuildConfig|R)([^\w])/, {
                  wholeString, prefix, className, suffix ->
                    "${prefix}${packageName}.${className}${suffix}"
                })
      }
      packageImports = packages.collect {
        "// ${it.name}\n${interpolateDynamicValues(it.packageImportPath)}"
      }.join('\n')
      packageClassInstances = ",\n      " + packages.collect {
        interpolateDynamicValues(it.packageInstance)
      }.join(",\n      ")
    }

    String generatedFileContents = generatedFileContentsTemplate
{% raw %}
      .replace("{{ packageImports }}", packageImports)
{% endraw %}
{% raw %}
      .replace("{{ packageClassInstances }}", packageClassInstances)
{% endraw %}

    outputDir.mkdirs()
    final FileTreeBuilder treeBuilder = new FileTreeBuilder(outputDir)
    treeBuilder.file(generatedFileName).newWriter().withWriter { w ->
      w << generatedFileContents
    }
  }
```

这个函数同样也是利用到了reactNativeModules，最终结果是生成了一个文件PackageList.java,路径是`/android/build/generated/rn/src/main/java/com/facebook/react/PackageList.java`\
来看看这个`PackageList.java`文件长什么样

```java
package com.facebook.react;

import android.app.Application;
import android.content.Context;
import android.content.res.Resources;

import com.facebook.react.ReactPackage;
import com.facebook.react.shell.MainPackageConfig;
import com.facebook.react.shell.MainReactPackage;
import java.util.Arrays;
import java.util.ArrayList;


import xx.xxPackage;

public class PackageList {
    private Application application;
    private ReactNativeHost reactNativeHost;
    private MainPackageConfig mConfig;

    public PackageList(ReactNativeHost reactNativeHost) {
        this(reactNativeHost, null);
    }

    public PackageList(Application application) {
        this(application, null);
    }

    public PackageList(ReactNativeHost reactNativeHost, MainPackageConfig config) {
        this.reactNativeHost = reactNativeHost;
        mConfig = config;
    }

    public PackageList(Application application, MainPackageConfig config) {
        this.reactNativeHost = null;
        this.application = application;
        mConfig = config;
    }

    private ReactNativeHost getReactNativeHost() {
        return this.reactNativeHost;
    }

    private Resources getResources() {
        return this.getApplication().getResources();
    }

    private Application getApplication() {
        if (this.reactNativeHost == null) return this.application;
        return this.reactNativeHost.getApplication();
    }

    private Context getApplicationContext() {
        return this.getApplication().getApplicationContext();
    }

    public ArrayList<ReactPackage> getPackages() {
        return new ArrayList<>(Arrays.<ReactPackage>asList(
            new MainReactPackage(mConfig),
            new xxPackage()
        ));
    }
}
```

定义了一个PackageList类，重点是`getPackages()`方法,返回所有的package，那么这个类的主要作用是什么呢，答案下面揭晓

### 3.在`MainApplication.java`里配置package依赖包

看一下现在的`MainApplication.java`代码

```java
...
import com.facebook.react.PackageList;
...
@Override
protected List<ReactPackage> getPackages() {
    @SuppressWarnings("UnnecessaryLocalVariable")
    List<ReactPackage> packages = new PackageList(this).getPackages();
    // Packages that cannot be autolinked yet can be added manually here, for example:
    // packages.add(new MyReactNativePackage());
    return packages;
}
...
```

可以看出，一个叫`com.facebook.react.PackageList`包被导入了，这个包就是上一步生成的`PackageList.java`，通过`sourceSets`指定源文件目录被编译和识别。\
`MainApplication.java`的`getPackages()`调用了`PackageList`类实例的`getPackages()`，将返回的所有包封装成一个数组返回，这个对应了 之前的

```java
@Override
protected List<ReactPackage> getPackages() {
    return Arrays.<ReactPackage>asList(
        new xx依赖Package()
    );
}
```

至此，autolink的大概过程就解析完毕了，可以看出，每一个过程都能与之前的react-native link形成对应，autolink通过加载gradle插件将link过程自动化，从而省略了手动link这一步骤

## 三、autolink失败的原因

回到最开始的问题，为什么不规范的第三方库会导致link失败呢？\
这里我们引入两个库测试，一个是文件结构规范的`@react-native-community/async-storage`库,另一个是文件结构不规范(android和ios文件夹在lib目录下)的`react-native-amap-geolocation`库\
我们回到在autolink过程中一直被用到的依赖对象数组`reactNativeModules`的构建过程，看下它构建后的值

```json
[
  [nameCleansed:react-native-community_async-storage, 
  androidSourceDir:E:\xx项目\node_modules\@react-native-community\async- 
  storage\android,
  packageInstance:new AsyncStoragePackage(), 
  name:@react-native-community/async-storage,
  packageImportPath:import 
  com.reactnativecommunity.asyncstorage.AsyncStoragePackage;], 
]
```

可以发现，这个数组只包含了`@react-native-community/async-storage`库，而没有`react-native-amap-geolocation`,从而导致通过此数组做的各种配置少了这个库，这就是autolink失败的原因了。\
我们再继续探究下去，看看更详细的原因\
`reactNativeModules`的值是由`getReactNativeConfig()`方法返回的

```java
...
ArrayList<HashMap<String, String>> getReactNativeConfig() {
    if (this.reactNativeModules != null) return this.reactNativeModules

        ArrayList<HashMap<String, String>> reactNativeModules = new ArrayList<HashMap<String, String>>()
    def cliResolveScript = "console.log(require('react-native/cli').bin);"
    String[] nodeCommand = ["node", "-e", cliResolveScript]
    def cliPath = this.getCommandOutput(nodeCommand, this.root) 
    //E:\xx项目\node_modules\@react-native-community\cli\build\bin.js
    String[] reactNativeConfigCommand = ["node", cliPath, "config"]
    def reactNativeConfigOutput = this.getCommandOutput(reactNativeConfigCommand, this.root)

    def json
    try {
        json = new JsonSlurper().parseText(reactNativeConfigOutput)
    } catch (Exception exception) {
        throw new Exception("Calling `${reactNativeConfigCommand}` finished with an exception. Error message: ${exception.toString()}. Output: ${reactNativeConfigOutput}");
    }
    def dependencies = json["dependencies"]
    def project = json["project"]["android"]

    if (project == null) {
        throw new Exception("React Native CLI failed to determine Android project configuration. This is likely due to misconfiguration. Config output:\n${json.toMapString()}")
    }

    dependencies.each { name, value ->
                        def platformsConfig = value["platforms"];
                        def androidConfig = platformsConfig["android"]
                        //  this.logger.error("${value}") 输出日志
                        if (androidConfig != null && androidConfig["sourceDir"] != null) {
                            this.logger.info("${LOG_PREFIX}Automatically adding native module '${name}'")

                            HashMap reactNativeModuleConfig = new HashMap<String, String>()
                            reactNativeModuleConfig.put("name", name)
                            reactNativeModuleConfig.put("nameCleansed", name.replaceAll('[~*!\'()]+', '_').replaceAll('^@([\\w-.]+)/', '$1_'))
                            reactNativeModuleConfig.put("androidSourceDir", androidConfig["sourceDir"])
                            reactNativeModuleConfig.put("packageInstance", androidConfig["packageInstance"])
                            reactNativeModuleConfig.put("packageImportPath", androidConfig["packageImportPath"])
                            this.logger.trace("${LOG_PREFIX}'${name}': ${reactNativeModuleConfig.toMapString()}")

                            reactNativeModules.add(reactNativeModuleConfig)
                        } else {
                            this.logger.info("${LOG_PREFIX}Skipping native module '${name}'")
                        }
                      }

    return [reactNativeModules, json["project"]["android"]["packageName"]];
}
...
```

通过打log，可以得知，这个方法执行了`"node E:\xx项目\node_modules\@react-native-community\cli\build\bin.js config "` 命令，将返回的数据封装成一个对象，再遍历这个对象的`dependecies`数组，如果某一项的`.platforms.android.sourceDir`不为null，则会将它的android模块的各项信息封装一个对象，作为最后返回结果`reactNativeModules`的其中一项，我们来打印一下dependecies中的`@react-native-community/async-storage`库这一项的信息：

```json
[root:E:\work\v-next\orion-app\node_modules\@react-native-community\async-storage, 
 name:@react-native-community/async-storage, 
 platforms:[
   ios: [
       sourceDir:E:\xx项目\node_modules\@react-native-community\async-storage\ios, 
       folder:E:\xx项目\node_modules\@react-native-community\async-storage, 
       pbxprojPath:E:\work\xx项目\node_modules\@react-native-community\async-storage\ios\RNCAsyncStorage.xcodeproj\project.pbxproj, 
       podfile:null, 
       podspecPath:E:\xx项目\node_modules\@react-native-community\async-storage\RNCAsyncStorage.podspec, 
       projectPath:E:\xx项目\node_modules\@react-native-community\async-storage\ios\RNCAsyncStorage.xcodeproj, 
       projectName:RNCAsyncStorage.xcodeproj, 
       libraryFolder:Libraries, 
       sharedLibraries:[], 
       plist:[], 
       scriptPhases:[]
       ], 
   android:[
        sourceDir:E:\xx项目\node_modules\@react-native-community\async-storage\android, 
        folder:E:\xx项目\node_modules\@react-native-community\async-storage, 
        packageImportPath:import com.reactnativecommunity.asyncstorage.AsyncStoragePackage;, 
        packageInstance:new AsyncStoragePackage()
        ]
  ], 
 assets:[], 
 hooks:[:], 
 params:[]
]
```

再来打印`react-native-amap-geolocation`库这一项的信息

```json
[root:E:\work\v-next\orion-app\node_modules\react-native-amap-geolocation, 
 name:react-native-amap-geolocation, 
 platforms:[
   ios: [
         sourceDir:E:\work\v-next\orion-app\node_modules\react-native-amap-geolocation\ios, 
         folder:E:\work\v-next\orion-app\node_modules\react-native-amap-geolocation, 
         pbxprojPath:E:\work\v-next\orion-app\node_modules\react-native-amap-geolocation\ios\RNAMapGeolocation.xcodeproj\project.pbxproj, 
         podfile:null, 
         podspecPath:null, 
         projectPath:E:\work\v-next\orion-app\node_modules\react-native-amap-geolocation\ios\RNAMapGeolocation.xcodeproj, 
         projectName:RNAMapGeolocation.xcodeproj, 
         libraryFolder:Libraries,
         sharedLibraries:[], 
         plist:[],
         scriptPhases:[]
        ], 
   android:null
 ], 
 assets:[], 
 hooks:[:], 
 params:[]
]
```

`android`属性居然为null！这就导致了，最终形成的`reactNativeModules`不会有`react-native-amap-geolocation`这个库\
为什么会出现这样的情况呢？我们继续探究下去\
显而易见，原因出在执行的node命令当中，我们前往这个`E:\xx项目\node_modules\@react-native-community\cli\build\bin.js`文件

```jsx
#!/usr/bin/env node
"use strict";

require("./tools/gracefulifyFs");

var _ = require("./"); // 引入目录下的`index.js`
(0, _.run)(); 

//# sourceMappingURL=bin.js.map
```

可以看出此文件引入了目录下的`index.js文件`,并调用其中的run方法(注意：(0,func)(params)这种形式的写法可以看成是this指向全局windows对象的func(params))\
我们看下`index.js`里的`run`方法

```php
var _config = _interopRequireDefault(require("./tools/config"));

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

async function run() {
  try {
    await setupAndRun();
  } catch (e) {
    handleError(e);
  }
}
async function setupAndRun() {
  // Commander is not available yet
  // when we run `config`, we don't want to output anything to the console. We
  // expect it to return valid JSON
  if (process.argv.includes('config')) {
    _cliTools().logger.disable();
  }

  _cliTools().logger.setVerbose(process.argv.includes('--verbose')); // We only have a setup script for UNIX envs currently


  if (process.platform !== 'win32') {
    const scriptName = 'setup_env.sh';

    const absolutePath = _path().default.join(__dirname, '..', scriptName);

    try {
      _child_process().default.execFileSync(absolutePath, {
        stdio: 'pipe'
      });
    } catch (error) {
      _cliTools().logger.warn(`Failed to run environment setup script "${scriptName}"\n\n${_chalk().default.red(error)}`);

      _cliTools().logger.info(`React Native CLI will continue to run if your local environment matches what React Native expects. If it does fail, check out "${absolutePath}" and adjust your environment to match it.`);
    }
  }

  for (const command of _commands.detachedCommands) {
    attachCommand(command);
  }

  try {
    const ctx = (0, _config.default)();

    _cliTools().logger.enable();

    for (const command of [..._commands.projectCommands, ...ctx.commands]) {
      attachCommand(command, ctx);
    }
  } catch (e) {
    _cliTools().logger.enable();

    _cliTools().logger.debug(e.message);

    _cliTools().logger.debug('Failed to load configuration of your project. Only a subset of commands will be available.');
  }

  _commander().default.parse(process.argv);

  if (_commander().default.rawArgs.length === 2) {
    _commander().default.outputHelp();
  } // We handle --version as a special case like this because both `commander`
  // and `yargs` append it to every command and we don't want to do that.
  // E.g. outside command `init` has --version flag and we want to preserve it.


  if (_commander().default.args.length === 0 && _commander().default.rawArgs.includes('--version')) {
    console.log(pkgJson.version);
  }
}
```

重点看`const ctx = (0, _config.default)();`这行代码,查看`_config`的值可知，这行代码是调用了\
`./tools/config/index.js`里的`default`方法\
我们继续前往`./tools/config/index.js`文件...

```jsx
function loadConfig(projectRoot = (0, _findProjectRoot.default)()) {
  let lazyProject;
  const userConfig = (0, _readConfigFromDisk.readConfigFromDisk)(projectRoot);
  const initialConfig = {
    root: projectRoot,

    get reactNativePath() {
      return userConfig.reactNativePath ? _path().default.resolve(projectRoot, userConfig.reactNativePath) : (0, _resolveReactNativePath.default)(projectRoot);
    },

    dependencies: userConfig.dependencies,
    commands: userConfig.commands,

    get assets() {
      return (0, _findAssets.default)(projectRoot, userConfig.assets);
    },

    platforms: userConfig.platforms,

    get project() {
      if (lazyProject) {
        return lazyProject;
      }

      lazyProject = {};

      for (const platform in finalConfig.platforms) {
        const platformConfig = finalConfig.platforms[platform];

        if (platformConfig) {
          lazyProject[platform] = platformConfig.projectConfig(projectRoot, userConfig.project[platform] || {});
        }
      }

      return lazyProject;
    }

  };
  const finalConfig = Array.from(new Set([...Object.keys(userConfig.dependencies), ...(0, _findDependencies.default)(projectRoot)])).reduce((acc, dependencyName) => {
    const localDependencyRoot = userConfig.dependencies[dependencyName] && userConfig.dependencies[dependencyName].root;
    let root;
    let config;

    try {
      root = localDependencyRoot || (0, _resolveNodeModuleDir.default)(projectRoot, dependencyName);
      config = (0, _readConfigFromDisk.readDependencyConfigFromDisk)(root);
    } catch (error) {
      _cliTools().logger.warn((0, _cliTools().inlineString)(`
          Package ${_chalk().default.bold(dependencyName)} has been ignored because it contains invalid configuration.

          Reason: ${_chalk().default.dim(error.message)}`));

      return acc;
    }

    const isPlatform = Object.keys(config.platforms).length > 0;
    return (0, _assign.default)({}, acc, {
      dependencies: (0, _assign.default)({}, acc.dependencies, {
        get [dependencyName]() {
          return getDependencyConfig(root, dependencyName, finalConfig, config, userConfig, isPlatform);
        }

      }),
      commands: [...acc.commands, ...config.commands],
      platforms: { ...acc.platforms,
        ...config.platforms
      }
    });
  }, initialConfig);
  return finalConfig;
}

var _default = loadConfig;
exports.default = _default;
```

可以看出，`default`方法，其实就是`loadConfig`方法，`loadConfig`方法最终返回一个叫`finalConfig`的属性，由代码可知，`dependecies`属性是通过调用`getDependencyConfig(root, dependencyName, finalConfig, config, userConfig, isPlatform)`来赋值的，我们前往这个`getDependencyConfig`方法

```jsx
function getDependencyConfig(root, dependencyName, finalConfig, config, userConfig, isPlatform) {
  return (0, _merge.default)({
    root,
    name: dependencyName,
    platforms: Object.keys(finalConfig.platforms).reduce((dependency, platform) => {
      const platformConfig = finalConfig.platforms[platform];
      dependency[platform] = // Linking platforms is not supported
      isPlatform || !platformConfig ? null : platformConfig.dependencyConfig(root, config.dependency.platforms[platform]);
      return dependency;
    }, {}),
    assets: (0, _findAssets.default)(root, config.dependency.assets),
    hooks: config.dependency.hooks,
    params: config.dependency.params
  }, userConfig.dependencies[dependencyName] || {});
}
```

重点是`platformConfig.dependencyConfig`这个方法，这个方法由前面的下面这段代码添加上的：

```plain
config = (0, _readConfigFromDisk.readDependencyConfigFromDisk)(root);
```

我们不再过多地叙述这段代码的具体实现，只需要知道它通过给定的root路径定位到了`node_modules/react-native`文件夹下的`react-native-config.js`

```jsx
const ios = require('@react-native-community/cli-platform-ios');
const android = require('@react-native-community/cli-platform-android');

module.exports = {
  commands: [...ios.commands, ...android.commands],
  platforms: {
    ios: {
      linkConfig: ios.linkConfig,
      projectConfig: ios.projectConfig,
      dependencyConfig: ios.dependencyConfig,
    },
    android: {
      linkConfig: android.linkConfig,
      projectConfig: android.projectConfig,
      dependencyConfig: android.dependencyConfig,
    },
  },
  /**
   * Used when running RNTester (with React Native from source)
   */
  reactNativePath: '.',
  project: {
    ios: {
      project: './RNTester/RNTesterPods.xcworkspace',
    },
    android: {
      sourceDir: './RNTester',
    },
  },
};
```

并提取其中的dependencyConfig属性，我们前往`@react-native-community/cli-platform-android`

```tsx
...
Object.defineProperty(exports, "dependencyConfig", {
  enumerable: true,
  get: function () {
    return _config.dependencyConfig;
  }
});

...
var _config = require("./config");
···
```

不多说什么，我们继续前往`./config`

```csharp
...
function dependencyConfig(root, userConfig = {}) {
  const src = userConfig.sourceDir || (0, _findAndroidDir.default)(root);

  if (!src) {
    return null;
  }

  const sourceDir = _path().default.join(root, src);

  const manifestPath = userConfig.manifestPath ? _path().default.join(sourceDir, userConfig.manifestPath) : (0, _findManifest.default)(sourceDir);

  if (!manifestPath) {
    return null;
  }

  const manifest = (0, _readManifest.default)(manifestPath);
  const packageName = userConfig.packageName || getPackageName(manifest);
  const packageClassName = (0, _findPackageClassName.default)(sourceDir);
  /**
   * This module has no package to export
   */

  if (!packageClassName) {
    return null;
  }

  const packageImportPath = userConfig.packageImportPath || `import ${packageName}.${packageClassName};`;
  const packageInstance = userConfig.packageInstance || `new ${packageClassName}()`;
  return {
    sourceDir,
    folder: root,
    packageImportPath,
    packageInstance
  };
}
```

可以看到，如果userConfig.sourceDir为空，就是用户未指定资源路径，就会调用\
`_findAndroidDir.default(root)`方法来设置\
我们继续前往这个方法（放心，这次是最后一次了。。）

```php
function findAndroidDir(root) {
  if (_fs().default.existsSync(_path().default.join(root, 'android'))) {
    return 'android';
  }

  return null;
}
```

我们打印一下root的值

// react-native-amap-geolocation\
E:\xx项目\node\_modules\react-native-amap-geolocation\
// @react-native-community\async-storage\
E:\xx项目\node\_modules@react-native-community\async-storage

可以看到root的值都是`.\node_modules\+库名`这种形式，而`findAndroidDir`调用`existsSync()`来判断`${root}/android`这个目录是否存在，很显然，由于`react-native-amap-geolocation`的android目录是`${root}/lib/android`所以导致这个方法返回了null, `dependencyConfig`方法也返回了null，层层往上，最终导致`reactNativeModules`中的这一个库的android属性项为空\
真相终于找到了

## 四、如何解决

原因找到了，除了更改第三方库文件结构这种方法外，有没有什么比较优雅的方式呢？答案是有的，有两种

* 1、跟之前一样，react-native link ‘目标库’，不走autolink的流程
* 2、在项目根目录下的`react-native-config.js`配置文件下手动指定目标库的root路径

```java
const path = require('path')
module.exports = {
  dependencies: {
    'react-native-amap-geolocation': {
      root: path.join(__dirname, 'node_modules/react-native-amap-geolocation/lib'),//注意，不能用相对路径
    },
  },
}
```


