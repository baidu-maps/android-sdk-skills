# 工程配置

**边界**：AndroidManifest、权限、定位 Service、混淆；Gradle 依赖见 [gradle.md](gradle.md)。

## AndroidManifest

### 配置 AK

在 `<application>` 内添加 meta-data：

```xml
<meta-data
    android:name="com.baidu.lbsapi.API_KEY"
    android:value="您的AK" />
```

**控制台配置**：在百度开放平台「应用管理 - 我的应用」创建应用，类型选 **Android SDK**；包名与 `build.gradle` 中 `applicationId` 完全一致；SHA1 填写调试/发布签名对应值。

### 权限

```xml
<!-- 网络定位（必须） -->
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.INTERNET" />

<!-- 精确/粗略位置（必须，Android 6.0+ 还需运行时申请） -->
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />

<!-- 可选：读取手机状态（辅助定位） -->
<uses-permission android:name="android.permission.READ_PHONE_STATE" />

<!-- Android 10+ 后台定位（仅在必要时申请） -->
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
```

自 Android 6.0 起，`ACCESS_FINE_LOCATION` 与 `ACCESS_COARSE_LOCATION` 为危险权限，需运行时申请（见 [basic-location.md](basic-location.md)）。Android 10+ 的后台定位（`ACCESS_BACKGROUND_LOCATION`）需单独申请且需说明理由。

### 定位 Service

在 `<application>` 内声明定位服务（具体类名以当前 SDK 版本文档为准）：

```xml
<service
    android:name="com.baidu.location.f"
    android:enabled="true"
    android:process=":remote" />
```

### 前台 Service（后台持续定位）

Android 8.0+ 对后台定位有严格限制，持续定位建议使用前台 Service，并在通知栏显示定位状态：

```xml
<uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_LOCATION" />
```

## 混淆（ProGuard）

```
-keep class com.baidu.** { *; }
-keep class vi.com.** { *; }
-dontwarn com.baidu.**
```

在 `buildTypes.release` 中引用：

```gradle
buildTypes {
    release {
        minifyEnabled true
        proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
    }
}
```

## 集成经验与常见问题

**排查顺序**（建议遵守）：先确认 **AK/包名/SHA1** 与控制台一致；再确认 **隐私接口** 在 `LocationClient` 构造前调用；仍失败时查 **Logcat**（关键字：`BaiduLocation`、`locType`）。

| 问题 | 原因 / 建议 |
|------|-------------|
| **定位一直失败（locType=162）** | 检查 AK 是否正确、运行时权限是否已授予、网络是否可用；见 [errorcode.md](errorcode.md) |
| **构造 LocationClient 抛异常** | `LocationClient.setAgreePrivacy(true)` 未在构造前调用，见 [overview.md](overview.md) |
| **地址字段为空** | 未调用 `setIsNeedAddress(true)` 或网络不可用 |
| **后台定位中断** | Android 8+ 后台限制，需使用前台 Service 持有 LocationClient |
| **包名签名不一致** | 控制台 AK 绑定的包名/SHA1 与当前构建不一致，重新申请或更新控制台配置 |
