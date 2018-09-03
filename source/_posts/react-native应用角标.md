---
title: react-native应用角标
date: 2018-08-03 15:04:10
tags: [react-native,android]
---

## 角标
应用角标是iOS的一个特色,原生Android并不支持。
第三方深度定制过的android系统有此类功能,如：三星、小米、魅族、华为等。
各方产商定制方法各不相同,我选择使用第三方开源的项目实现。

## 环境
```json
    "react": "16.3.1",
    "react-native": "0.55.4",
    "ShortcutBadger":"1.1.22" // 这个不是 package.json 的 dependencies
```

## ShortcutBadger 引入
1.下载 
通过 git
`git clone https://github.com/leolin310148/ShortcutBadger.git`
或 直接下载zip文件 
[ShortcutBadger github地址](https://github.com/leolin310148/ShortcutBadger)
2.解压 将文件放入工程 android 文件夹中
![图1](./目录结构.png)
也可将文件放在其他位置

<!--more-->

## 文件修改
1.android/settings.gradle
文件中加入
`include ':ShortcutBadger'`
![图2](./settings1.png)
如果 ShortcutBadger 未放在 android 文件夹中
需要添加
`project(':ShortcutBadger').projectDir = new File(rootProject.projectDir, '此处为相对于 settings.gradle 文件的地址')`

2.android/build.gradle
将 mavenCentral 添加到构建脚本。
```
    ...
    allprojects {
        repositories {
            mavenCentral()
            ...
        }
    }
    ...
```
![图3](./build.gradle.png)

3.android/app/build.gradle
为 ShortcutBadger 添加依赖项
```
    ...
    dependencies {
        compile "me.leolin:ShortcutBadger:1.1.22@aar"
        ...
    }
    ....
```
![图4](./app-build.gradle.png)

4.android/app/src/main/AndroidManifest.xml
添加 app 权限
```
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.jgapp">
        ....
        <uses-permission android:name="com.sec.android.provider.badge.permission.READ" />
        <uses-permission android:name="com.sec.android.provider.badge.permission.WRITE" />
        ...
    </manifest>
```


## 实现原生与ReactNative通信
android 通过 Android Studio 编辑(我是这么做的,当然其他编辑器也可以)
![图5](./新建类.png)
1.新建 BadgeModule(类) 继承 ReacContextBaseJavaModule
![图6](./新建类1.png)
```java
    package com.jgapp;//包名 请更改

    import com.facebook.react.bridge.ReactApplicationContext;
    import com.facebook.react.bridge.ReactContextBaseJavaModule;
    import com.facebook.react.bridge.ReactMethod;

    import android.content.Context;
    import android.widget.Toast;
    //引用外部类 me.leolin.shortcutbadger(包名)ShortcutBadger(类名)
    import me.leolin.shortcutbadger.ShortcutBadger;

    public class BadgeModule extends ReactContextBaseJavaModule {

        public BadgeModule(ReactApplicationContext reactContext) {
            super(reactContext);
        }

        /**
        * 这个返回的字符串是我们js端调用时会用到的
        */
        @Override
        public String getName() {
            return "Badge";
        }

        /**
        *这个方法是我们js端调用的方法，其中的参数可以从js端传过来  如这里我们js端可以类似  Badge.showBadge(2)来调用这个方法
        */
        @ReactMethod
        public void showBadge(int badgeNum) {
            boolean success = ShortcutBadger.applyCount(getCurrentActivity(), badgeNum);
            //toast 框 下面两行可以删除
            Toast.makeText(getCurrentActivity(), "Set count=" + badgeNum + ", success=" + success, Toast.LENGTH_SHORT).show();
            System.out.println(success);
        }
        /**
        *这个方法是我们js端调用的方法，其中的参数可以从js端传过来  如这里我们js端可以类似  Badge.removeBadge()来调用这个方法
        */
        @ReactMethod
        public void removeBadge() {
            boolean isSuccess = ShortcutBadger.removeCount(getCurrentActivity());
            //toast 框 下面两行可以删除
            Toast.makeText(getCurrentActivity(),  "success=" + isSuccess, Toast.LENGTH_SHORT).show();
            System.out.println(isSuccess);
        }
    }
```

2.定义 BadgePackage(包) 继承 ReactPackage
```java
    package com.jgapp;

    import com.facebook.react.bridge.ReactApplicationContext;
    import com.facebook.react.ReactPackage;
    import com.facebook.react.uimanager.ViewManager;
    import com.facebook.react.bridge.NativeModule;

    import java.util.ArrayList;
    import java.util.Arrays;
    import java.util.Collections;
    import java.util.List;

    public class BadgePackage implements ReactPackage {

        @Override
        public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
            return Collections.emptyList();
        }

        @Override
        public List<NativeModule> createNativeModules(
                ReactApplicationContext reactContext) {
            List<NativeModule> modules = new ArrayList<>();
            // 这里就是自己写的module
            modules.add(new BadgeModule(reactContext));

            return modules;
        }
    }
```

3.在 MainApplication 里面注册 BadgePackage
```java
    package com.jgapp;

    import android.app.Application;

    import com.facebook.react.ReactApplication;
    import com.beefe.picker.PickerViewPackage;
    import com.imagepicker.ImagePickerPackage;
    import com.oblador.vectoricons.VectorIconsPackage;
    import com.facebook.react.ReactNativeHost;
    import com.facebook.react.ReactPackage;
    import com.facebook.react.shell.MainReactPackage;
    import com.facebook.soloader.SoLoader;

    import java.util.Arrays;
    import java.util.List;

    public class MainApplication extends Application implements ReactApplication {

    private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
        @Override
        public boolean getUseDeveloperSupport() {
        return BuildConfig.DEBUG;
        }

        @Override
        protected List<ReactPackage> getPackages() {
        return Arrays.<ReactPackage>asList(
            new MainReactPackage(),
            // 以下三个为react-native的插件相信你认识
            new PickerViewPackage(),
            new ImagePickerPackage(),
            new VectorIconsPackage(),
            // 此处为添加的package
            new BadgePackage()
        );
        }

        @Override
        protected String getJSMainModuleName() {
        return "index";
        }
    };

    @Override
    public ReactNativeHost getReactNativeHost() {
        return mReactNativeHost;
    }

    @Override
    public void onCreate() {
        super.onCreate();
        SoLoader.init(this, /* native exopackage */ false);
    }
    }

```

## ReactNative 中调用
```javascript
    import { NativeModules } from 'react-native';

    const Badge = NativeModules.Badge;

    Badge.showBadge(5);
```
![图7](./badge.png)
其中 `Badge.removeCount()` 方法和 `Badge.showBadge(0)` 其实是一样的 因为...
![图8](./ShortcutBadger2.png)

## 查看 ShortcutBadger 可使用的方法
查看 `ShortcutBadger.java` 
![图9](./目录结构2.png)
![图10](./ShortcutBadger.png)

## 参考

[react-native 结合原生安卓实现角标](https://blog.csdn.net/lqx_sunhan/article/details/80377482)
[react-native之module的使用](https://blog.csdn.net/u014041033/article/details/50610873)
