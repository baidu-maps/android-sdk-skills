# 百度定位 Android SDK 参考索引

按需选下表文档；通用规则（隐私合规、AK、生命周期、监听先于启动、坐标系）见 [SKILL.md](../SKILL.md)。

## 按需求选文档

| 你要做的 | 用到的文档 |
|----------|------------|
| Gradle 依赖、基础包与全量包区别、版本说明 | [gradle.md](gradle.md) |
| AndroidManifest（AK、权限、定位 Service）、混淆规则 | [project-config.md](project-config.md) |
| 集成步骤、AK 申请、隐私合规、LocationClient 初始化 | [overview.md](overview.md) |
| 单次定位、连续定位、BDLocation 字段解析、定位类型 | [basic-location.md](basic-location.md) |
| LocationClientOption 全参数配置、坐标系、定位模式、场景化示例 | [location-config.md](location-config.md) |
| 圆形/多边形地理围栏、进入/离开/停留事件 | [geofence.md](geofence.md) |
| 坐标转地址、分层地址字段、POI 周边检索 | [geocode.md](geocode.md) |
| locType 含义、定位失败排查、鉴权错误码 | [errorcode.md](errorcode.md) |
| 类与包速查 | [class-reference.md](class-reference.md) |

## 常见组合

| 需求 | 文档组合 |
|------|----------|
| 快速获取当前位置（单次定位） | gradle → project-config → overview → basic-location |
| 持续获取位置（轨迹/导航） | gradle → project-config → overview → basic-location + location-config（省电/高精度模式） |
| 到店/安全区域监控 | overview → geofence（配合前台 Service 保活） |
| 坐标 → 可读地址 | basic-location + geocode（setIsNeedAddress + setNeedNewVersionRgc） |
| 室内定位 | location-config（setOpenIndoorMap）+ basic-location（locType 62） |
| 定位失败排查 | errorcode + project-config（AK/包名/签名排查）+ Logcat |

## 文档边界（一览）

| 文档 | 内容 | 边界 |
|------|------|------|
| [gradle.md](gradle.md) | 依赖坐标、基础/全量包区别、NDK、版本 | 构建报错见 project-config |
| [project-config.md](project-config.md) | AndroidManifest、权限、定位 Service、混淆 | 仅百度定位 SDK 相关配置 |
| [overview.md](overview.md) | 集成流程、AK 申请、隐私合规、LocationClient 核心用法 | 具体参数见 location-config |
| [basic-location.md](basic-location.md) | 单次/连续定位代码、BDLocation 字段、运行时权限 | 参数调优见 location-config |
| [location-config.md](location-config.md) | LocationClientOption 全参数、场景化配置 | 定位结果字段见 basic-location |
| [geofence.md](geofence.md) | BDGeofenceClient、圆形/多边形围栏、广播接收 | 不含服务端围栏 |
| [geocode.md](geocode.md) | SDK 内置逆地理、地址字段、POI 列表 | 正向地理编码（地址→坐标）不在定位 SDK 范围 |
| [errorcode.md](errorcode.md) | locType 错误值、鉴权错误码 | 仅定位 SDK 相关 |
| [class-reference.md](class-reference.md) | 类与包速查 | 用法与示例见各功能文档 |

## 能力一览（技能内已覆盖）

- **基础定位**：单次定位、连续定位、GPS/Wi-Fi/基站融合、海外 WGS84。
- **定位配置**：LocationClientOption 精度模式、坐标系、扫描间隔、GPS 开关、超时、室内开关、地址信息、POI 开关。
- **地理围栏**：圆形围栏、多边形围栏、进入/离开/停留事件、PendingIntent 广播触发、围栏增删管理。
- **逆地理编码**：SDK 内置地址回调、分层地址字段（省市区街道门牌）、位置语义描述、周边 POI 列表。
- **室内定位**：开启室内定位模式、楼层信息（getFloor）。
- **工具与排查**：locType 定位类型速查、AK 鉴权错误码、常见集成问题排查。
