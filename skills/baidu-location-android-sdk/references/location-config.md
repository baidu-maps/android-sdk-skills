# LocationClientOption 配置参考

`LocationClientOption` 是定位参数的统一配置类；修改后调用 `mLocationClient.setLocOption(option)` 立即生效，无需重启。

## 定位模式

```java
// 高精度（默认）：GPS + 网络，精度最高，耗电最多
option.setLocationMode(LocationClientOption.LocationMode.Hight_Accuracy);

// 省电：仅网络（基站 + Wi-Fi），精度较低，耗电少
option.setLocationMode(LocationClientOption.LocationMode.Battery_Saving);

// 设备传感器：仅 GPS，无网络定位，室外精度高，室内无效
option.setLocationMode(LocationClientOption.LocationMode.Device_Sensors);
```

## 坐标系

| 值 | 坐标系 | 适用场景 |
|----|--------|----------|
| `"bd09ll"` | 百度经纬度（默认推荐） | 百度地图 SDK 展示、百度 POI 检索 |
| `"gcj02"` | 国测局坐标 | 高德地图、腾讯地图 |
| `"wgs84"` | GPS 原始坐标 | Google Maps、国际通用 |

```java
option.setCoorType("bd09ll");
```

不同坐标系之间存在偏移，混用会导致标注错位，务必全链路统一。

## 定位间隔

```java
option.setScanSpan(0);     // 0 = 单次定位
option.setScanSpan(2000);  // 连续定位，单位毫秒，最小 1000
```

## GPS 控制

```java
option.setOpenGps(true);   // 开启 GPS（高精度模式建议开启）
option.setOpenGps(false);  // 关闭 GPS（省电场景）
```

## 地址信息

```java
option.setIsNeedAddress(true);       // 返回详细地址（需联网）
option.setNeedNewVersionRgc(true);   // 新版逆地理编码（字段更丰富，推荐）
```

## 超时

```java
option.setHttpTimeOut(20000);    // HTTP 请求超时，默认 75000ms
option.setConnectTimeout(3000);  // 连接超时，默认 3000ms
```

## POI 周边信息

```java
option.setEnablePoi(true);        // 返回周边 POI 列表（getPoiList）
option.setPoiExtraInfo(true);     // 返回 POI 额外信息
```

## 室内定位

```java
option.setOpenIndoorMap(true);  // 开启室内定位（需全量定位包）
```

## 其他

```java
option.setLocationNotify(true);     // 仅位置变化时回调（减少无效回调）
option.setIgnoreKillProcess(true);  // 进程被杀时不停止后台定位 Service
```

## 场景化配置示例

### 高精度单次定位（启动时获取当前位置）

```java
LocationClientOption option = new LocationClientOption();
option.setLocationMode(LocationClientOption.LocationMode.Hight_Accuracy);
option.setScanSpan(0);
option.setOpenGps(true);
option.setCoorType("bd09ll");
option.setIsNeedAddress(true);
option.setNeedNewVersionRgc(true);
option.setHttpTimeOut(20000);
```

### 省电连续定位（后台轨迹记录）

```java
LocationClientOption option = new LocationClientOption();
option.setLocationMode(LocationClientOption.LocationMode.Battery_Saving);
option.setScanSpan(5000);
option.setOpenGps(false);
option.setCoorType("bd09ll");
option.setIsNeedAddress(false);    // 不需地址，减少网络开销
option.setLocationNotify(true);    // 仅位置变化时回调
```

### 高频实时导航（每秒更新）

```java
LocationClientOption option = new LocationClientOption();
option.setLocationMode(LocationClientOption.LocationMode.Hight_Accuracy);
option.setScanSpan(1000);          // 最小间隔
option.setOpenGps(true);
option.setCoorType("bd09ll");
option.setIsNeedAddress(false);    // 不需地址，降低延迟
```

### 室内定位

```java
LocationClientOption option = new LocationClientOption();
option.setLocationMode(LocationClientOption.LocationMode.Hight_Accuracy);
option.setScanSpan(2000);
option.setOpenIndoorMap(true);
option.setCoorType("bd09ll");
option.setIsNeedAddress(true);
```
