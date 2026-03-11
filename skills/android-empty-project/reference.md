# Android 空工程 - 补充参考

## Groovy 版 build 配置

若使用 Groovy 而非 Kotlin DSL，文件名改为 `settings.gradle`、`build.gradle`，语法示例：

**根 build.gradle：**
```groovy
buildscript {
    ext.kotlin_version = '1.9.20'
}
plugins {
    id 'com.android.application' version '8.2.0' apply false
    id 'org.jetbrains.kotlin.android' version '1.9.20' apply false
}
```

**app/build.gradle：**
```groovy
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
}
android {
    namespace 'com.example.myapp'
    compileSdk 34
    defaultConfig {
        applicationId 'com.example.myapp'
        minSdk 24
        targetSdk 34
        versionCode 1
        versionName '1.0'
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_17
        targetCompatibility JavaVersion.VERSION_17
    }
    kotlinOptions { jvmTarget = '17' }
}
dependencies {
    implementation 'androidx.core:core-ktx:1.12.0'
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.11.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}
```

---

## 自定义布局与主题

**res/layout/activity_main.xml：**
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/app_name" />
</LinearLayout>
```

MainActivity 中：`setContentView(R.layout.activity_main)`。

**res/values/themes.xml：**
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="Theme.MyApp" parent="Theme.AppCompat.Light.NoActionBar">
        <item name="colorPrimary">#3F51B5</item>
        <item name="colorPrimaryDark">#303F9F</item>
        <item name="colorAccent">#FF4081</item>
    </style>
</resources>
```

在 AndroidManifest 中引用：`android:theme="@style/Theme.MyApp"`

**注意：** 使用 `AppCompatActivity` 时，主题必须使用 `Theme.AppCompat` 系列，不能使用系统主题如 `Theme.Material.Light.NoActionBar`，否则会抛出 `java.lang.IllegalStateException: You need to use a Theme.AppCompat theme (or descendant) with this activity`。

---

## 常用权限

在 `AndroidManifest.xml` 的 `<manifest>` 下添加，例如：

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

危险权限需在运行时申请（Activity 中 `requestPermissions` 或使用 ActivityResult API）。

---

## 多模块

在 `settings.gradle.kts` 中增加：`include(":module-name")`。在根 `build.gradle.kts` 中无需对 library 模块单独 apply 插件，在 `module-name/build.gradle.kts` 中使用 `id("com.android.library")` 并配置 `namespace`、`compileSdk` 等。app 依赖：`implementation(project(":module-name"))`。

---

## JDK 版本要求

AGP（Android Gradle Plugin）8.x 版本需要 JDK 11 或更高版本，推荐使用 JDK 17。

| AGP 版本 | 最低 JDK 版本 | 推荐 JDK 版本 |
|----------|---------------|---------------|
| 8.2.x | JDK 11 | JDK 17 |
| 8.1.x | JDK 11 | JDK 17 |
| 8.0.x | JDK 11 | JDK 17 |
| 7.4.x | JDK 11 | JDK 11/17 |
| 7.3.x | JDK 11 | JDK 11/17 |

**配置方式：**

1. 环境变量方式（临时）：
   ```bash
   export JAVA_HOME=/path/to/jdk-17
   ```

2. gradle.properties 方式（推荐）：
   ```properties
   org.gradle.java.home=/Users/username/Library/Java/JavaVirtualMachines/jbr-17.0.14/Contents/Home
   ```

3. 命令行指定（临时）：
   ```bash
   JAVA_HOME=/path/to/jdk-17 ./gradlew assembleDebug
   ```

---

## local.properties 配置

首次编译前需配置 Android SDK 路径。创建 `local.properties` 文件：

```properties
sdk.dir=/Users/username/Library/Android/sdk
```

Windows 示例：
```properties
sdk.dir=C\:\\Users\\username\\AppData\\Local\\Android\\Sdk
```

**配置优先级：**
1. `local.properties` 中的 `sdk.dir`
2. 环境变量 `ANDROID_HOME`
3. 环境变量 `ANDROID_SDK_ROOT`

---

## 项目创建后首次编译

完成项目创建后，执行以下命令进行首次编译：

```bash
# 指定 JDK 17 并编译
JAVA_HOME=/path/to/jdk-17 ./gradlew assembleDebug

# 或在 gradle.properties 中配置后直接运行
./gradlew assembleDebug
```

首次编译会自动下载 Gradle、AGP 及依赖，可能需要较长时间。编译成功后会输出 APK：`app/build/outputs/apk/debug/app-debug.apk`。
