# 单次与连续定位

## 运行时权限（Android 6.0+）

在发起定位前，确保已获取位置权限：

```java
if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION)
        != PackageManager.PERMISSION_GRANTED) {
    ActivityCompat.requestPermissions(this,
        new String[]{
            Manifest.permission.ACCESS_FINE_LOCATION,
            Manifest.permission.ACCESS_COARSE_LOCATION
        }, REQUEST_CODE_LOCATION);
    return;
}
// 权限已获取，可以启动定位
mLocationClient.start();
```

## 单次定位

`setScanSpan(0)` 表示单次定位；收到回调后主动调用 `stop()`。

```java
LocationClientOption option = new LocationClientOption();
option.setScanSpan(0);              // 单次定位
option.setIsNeedAddress(true);      // 返回地址信息
option.setNeedNewVersionRgc(true);  // 使用新版逆地理编码
option.setCoorType("bd09ll");       // 百度坐标系
option.setOpenGps(true);

mLocationClient.setLocOption(option);
mLocationClient.registerLocationListener(new BDAbstractLocationListener() {
    @Override
    public void onReceiveLocation(BDLocation location) {
        if (location == null) return;
        handleLocation(location);
        mLocationClient.stop(); // 单次定位完成后停止
    }
});
mLocationClient.start();
```

## 连续定位

`setScanSpan` 设置间隔（最小 1000ms），不调用 `stop()` 则持续回调。

```java
LocationClientOption option = new LocationClientOption();
option.setScanSpan(2000);           // 每 2 秒定位一次
option.setIsNeedAddress(false);     // 连续定位一般不需要每次返回地址
option.setCoorType("bd09ll");
option.setOpenGps(true);

mLocationClient.setLocOption(option);
mLocationClient.start();

// 停止时
mLocationClient.stop();
```

## BDLocation 主要字段

| 字段 | 方法 | 说明 |
|------|------|------|
| 纬度 | `getLatitude()` | 取决于 `setCoorType` 设置的坐标系 |
| 经度 | `getLongitude()` | 同上 |
| 精度半径 | `getRadius()` | 单位：米，值越小越准确 |
| 定位类型 | `getLocType()` | 见下方类型表 |
| 详细地址 | `getAddrStr()` | 需 `setIsNeedAddress(true)` |
| 国家 | `getCountry()` | |
| 省份 | `getProvince()` | |
| 城市 | `getCity()` | |
| 城市编码 | `getCityCode()` | |
| 行政区划编码 | `getAdCode()` | |
| 区县 | `getDistrict()` | |
| 乡镇 | `getTown()` | |
| 街道 | `getStreet()` | |
| 门牌号 | `getStreetNumber()` | |
| 楼层 | `getFloor()` | 室内定位时有值 |
| 语义化描述 | `getLocationDescribe()` | 如"百度大厦附近50米" |
| 周边 POI | `getPoiList()` | 需 `setEnablePoi(true)` |
| 速度 | `getSpeed()` | 单位：km/h，GPS 模式有效 |
| 方向 | `getDirection()` | 0~360°，GPS 模式有效 |
| 定位时间 | `getTime()` | 时间字符串 |

## 定位类型（locType）

| 值 | 含义 |
|----|------|
| 61 | GPS 定位 |
| 62 | 室内定位 |
| 65 | 定位缓存（离线） |
| 66 | 离线定位 |
| 67 | 网络定位（基站） |
| 68 | 网络定位（Wi-Fi） |
| 161 | 网络融合定位 |
| 162 | 定位失败（见 [errorcode.md](errorcode.md)） |

## 注意

- `setScanSpan` 不能小于 1000ms，设为 0 为单次定位；小于 1000 时 SDK 自动修正为 1000。
- 连续定位期间修改 `LocationClientOption` 无需重启，直接 `setLocOption` 后立即生效。
- 海外位置仅支持 WGS84 坐标输出，国内支持 BD09LL/GCJ02。
