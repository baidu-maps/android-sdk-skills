# 类与包速查

百度定位 Android SDK 主要类速查表，用法与示例见各功能文档。

## 核心类

| 类名 | 包 | 用途 |
|------|----|------|
| `LocationClient` | `com.baidu.location` | 定位客户端，生命周期主入口 |
| `LocationClientOption` | `com.baidu.location` | 定位参数配置 |
| `BDLocation` | `com.baidu.location` | 定位结果，含坐标、地址、locType 等 |
| `BDAbstractLocationListener` | `com.baidu.location` | 定位回调抽象类，继承后实现 `onReceiveLocation` |
| `Poi` | `com.baidu.location` | BDLocation 中的 POI 信息 |

## 地理围栏类

| 类名 | 包 | 用途 |
|------|----|------|
| `BDGeofenceClient` | `com.baidu.location` | 地理围栏客户端 |
| `GeoFenceOption` | `com.baidu.location` | 围栏参数配置（圆形/多边形） |
| `GeoFence` | `com.baidu.location` | 围栏对象，含 fenceId 等信息 |
| `BDGeofenceClient.GeoFenceAddListener` | `com.baidu.location` | 围栏添加结果回调接口 |

## 枚举与常量

| 类名 | 说明 |
|------|------|
| `LocationClientOption.LocationMode` | 定位模式枚举：`Hight_Accuracy` / `Battery_Saving` / `Device_Sensors` |
| `BDGeofenceClient.GEOFENCE_IN` | 进入围栏触发类型（int 常量） |
| `BDGeofenceClient.GEOFENCE_OUT` | 离开围栏触发类型（int 常量） |
| `BDGeofenceClient.GEOFENCE_STAY` | 停留触发类型（int 常量） |
| `BDGeofenceClient.ACTION_RECEIVER_FENCE` | 围栏广播 Action 字符串 |
| `BDGeofenceClient.GEOFENCE_STATUS` | Intent extra key：触发类型 |
| `BDGeofenceClient.GEOFENCE_CUSTOM_ID` | Intent extra key：自定义业务 ID |
| `BDGeofenceClient.GEOFENCE_LATITUDE` | Intent extra key：触发时纬度 |
| `BDGeofenceClient.GEOFENCE_LONGITUDE` | Intent extra key：触发时经度 |
| `GeoFence.ADDGEOFENCE_SUCCESS` | 围栏添加成功码（0） |

## LocationClientOption 关键方法速查

| 方法 | 默认值 | 说明 |
|------|--------|------|
| `setLocationMode(LocationMode)` | Hight_Accuracy | 定位模式 |
| `setCoorType(String)` | `"gcj02"` | 坐标系，推荐 `"bd09ll"` |
| `setScanSpan(int)` | 0 | 定位间隔（ms），0=单次 |
| `setOpenGps(boolean)` | false | 是否开启 GPS |
| `setIsNeedAddress(boolean)` | false | 是否返回地址 |
| `setNeedNewVersionRgc(boolean)` | false | 新版逆地理编码 |
| `setEnablePoi(boolean)` | false | 返回周边 POI |
| `setPoiExtraInfo(boolean)` | false | POI 额外信息 |
| `setOpenIndoorMap(boolean)` | false | 室内定位 |
| `setLocationNotify(boolean)` | false | 仅位置变化时回调 |
| `setHttpTimeOut(long)` | 75000 | HTTP 超时（ms） |
| `setConnectTimeout(int)` | 3000 | 连接超时（ms） |
| `setIgnoreKillProcess(boolean)` | false | 进程被杀时不停止 Service |
