# 集成概述与核心初始化

SDK 集成推荐通过 **Gradle** 依赖（见 [gradle.md](gradle.md)）；AndroidManifest、混淆见 [project-config.md](project-config.md)。

## AK 申请

1. 登录[百度地图开放平台](https://lbs.baidu.com)，进入「控制台 - 应用管理 - 我的应用」，创建应用。
2. 应用类型选 **Android SDK**，填写：
   - **包名**：与 `build.gradle` 的 `applicationId` 完全一致。
   - **SHA1**：调试包用 `keytool -list -v -keystore ~/.android/debug.keystore`（默认口令 `android`）；发布包用 release keystore。
3. 将生成的 AK 配置到 AndroidManifest（见 [project-config.md](project-config.md)）。

## 隐私合规（v9.x+）

- 必须在用户**同意隐私政策后**，且在 `new LocationClient(context)` **之前**调用：
  ```java
  LocationClient.setAgreePrivacy(true);
  ```
- 未调用或传 `false` 时，`LocationClient` 构造会抛出异常，定位功能不可用。
- 推荐在 Application 的 `onCreate` 中，完成隐私弹窗逻辑后再初始化：

```java
public class MyApp extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        // 用户同意隐私政策后调用，必须在 new LocationClient 之前
        LocationClient.setAgreePrivacy(true);
    }
}
```

## LocationClient 初始化

```java
private LocationClient mLocationClient;

// 在 Activity / Fragment onCreate 中
try {
    mLocationClient = new LocationClient(getApplicationContext());
} catch (Exception e) {
    e.printStackTrace();
}

// 先注册监听，再设置参数，最后 start
mLocationClient.registerLocationListener(myListener);
mLocationClient.setLocOption(buildOption());
mLocationClient.start();
```

**注意**：`registerLocationListener` 必须在 `start()` 之前调用，否则可能收不到第一次回调。

## LocationListener 实现

继承 `BDAbstractLocationListener`，重写 `onReceiveLocation`：

```java
private final BDAbstractLocationListener myListener = new BDAbstractLocationListener() {
    @Override
    public void onReceiveLocation(BDLocation location) {
        if (location == null) return;
        int locType = location.getLocType();
        // locType == 61/161/67/68 等为成功，见 errorcode.md
        double lat = location.getLatitude();
        double lng = location.getLongitude();
    }
};
```

## 生命周期

```java
@Override
protected void onDestroy() {
    super.onDestroy();
    mLocationClient.stop();
    mLocationClient.unRegisterLocationListener(myListener);
}
```

后台持续定位时，建议在**前台 Service** 中持有 `LocationClient`，避免进程被系统回收导致定位中断。

## 核心类速查

| 用途 | 类 |
|------|----|
| 定位客户端 | `LocationClient` |
| 定位参数配置 | `LocationClientOption` |
| 定位结果 | `BDLocation` |
| 定位监听 | `BDAbstractLocationListener` |
| 地理围栏客户端 | `BDGeofenceClient` |
| 围栏配置 | `GeoFenceOption` |
| 围栏对象 | `GeoFence` |
