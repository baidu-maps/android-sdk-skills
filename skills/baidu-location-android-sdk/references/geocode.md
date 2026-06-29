# 逆地理编码

逆地理编码（坐标 → 地址）在百度定位 SDK 中通过 `LocationClientOption` 开启，在定位回调的 `BDLocation` 中直接读取，无需单独调用地图 API。

## 开启地址返回

```java
LocationClientOption option = new LocationClientOption();
option.setIsNeedAddress(true);       // 必须设为 true
option.setNeedNewVersionRgc(true);   // 新版逆地理编码，字段更丰富（推荐）
option.setEnablePoi(true);           // 同时返回周边 POI 列表
```

## 在回调中读取地址

```java
@Override
public void onReceiveLocation(BDLocation location) {
    if (location == null) return;

    // 完整地址
    String fullAddress = location.getAddrStr();        // 如：北京市海淀区上地十街10号

    // 分层地址
    String country    = location.getCountry();         // 国家
    String province   = location.getProvince();        // 省份
    String city       = location.getCity();            // 城市
    String cityCode   = location.getCityCode();        // 城市编码
    String adCode     = location.getAdCode();          // 行政区划编码（6位）
    String district   = location.getDistrict();        // 区县
    String town       = location.getTown();            // 镇/乡
    String street     = location.getStreet();          // 街道
    String streetNum  = location.getStreetNumber();    // 门牌号

    // 语义化描述
    String describe   = location.getLocationDescribe(); // 如"百度大厦附近50米"

    // 周边 POI 列表（需 setEnablePoi(true)）
    List<Poi> poiList = location.getPoiList();
    if (poiList != null) {
        for (Poi poi : poiList) {
            String name    = poi.getName();    // POI 名称
            String address = poi.getAddr();    // POI 地址
            String tags    = poi.getTags();    // POI 标签
        }
    }
}
```

## BDLocation 地址字段一览

| 方法 | 说明 | 前置条件 |
|------|------|----------|
| `getAddrStr()` | 完整地址字符串 | `setIsNeedAddress(true)` |
| `getCountry()` | 国家 | 同上 |
| `getProvince()` | 省份 | 同上 |
| `getCity()` | 城市 | 同上 |
| `getCityCode()` | 城市编码 | 同上 |
| `getAdCode()` | 行政区划编码（6位） | `setNeedNewVersionRgc(true)` |
| `getDistrict()` | 区县 | `setIsNeedAddress(true)` |
| `getTown()` | 镇/乡 | 同上 |
| `getStreet()` | 街道 | 同上 |
| `getStreetNumber()` | 门牌号 | 同上 |
| `getLocationDescribe()` | 语义化位置描述 | `setIsNeedAddress(true)` |
| `getPoiList()` | 周边 POI 列表 | `setEnablePoi(true)` |
| `getFloor()` | 楼层（室内） | `setOpenIndoorMap(true)` |

## 注意

- 地址信息依赖网络请求，网络不可用时字段为空；离线/缓存定位（locType=65/66）无地址。
- 地址精度受定位精度影响：`getRadius()` 越小地址越准确，建议 `radius < 100m` 时采信地址。
- `getAddrStr()` 与百度地图 Web API 逆地理编码结果来源相同，结果基本一致。
- 正向地理编码（地址 → 坐标）不在定位 SDK 范围内，需使用百度地图 SDK 的 `GeoCoder`。
