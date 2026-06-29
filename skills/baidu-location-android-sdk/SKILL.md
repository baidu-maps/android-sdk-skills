---
name: baidu-location-android-sdk
description: 百度定位 Android SDK 集成与开发规范。覆盖 AK 配置、隐私合规、单次/连续定位、LocationClientOption 配置、地理围栏、逆地理编码；输出符合隐私与 AK 配置要求的 Android 定位方案。在开发 Android 定位功能、集成百度定位 SDK、使用 LocationClient、地理围栏、逆地理编码时使用。
compatibility: Android, Gradle, Android Studio
---

# 百度定位 Android SDK

## 目标与边界

- **目标**：在 Android 工程中正确集成百度定位 SDK，给出符合隐私、AK、坐标系规范的定位实现方案。
- **负责**：单次/连续定位、LocationClientOption 参数配置、地理围栏（圆形/多边形）、逆地理编码、BDLocation 字段解析；以技能内 references 为准。
- **不负责**：百度地图 SDK 地图展示（见 baidu-map-android-sdk skill）、服务端逻辑、UI 视觉设计。

## 使用时机

满足其一即启用本技能：

- 关键词：百度定位、LocationClient、BDLocation、LocationClientOption、地理围栏、GeoFence、逆地理编码、BD09LL、坐标系
- 需求类型：获取当前位置、连续定位、轨迹记录、进出区域监控、坐标转地址、室内定位

**按需加载**：先根据需求在 [reference.md](references/reference.md) 中选定文档，再引用对应 references 内容；需求含糊时先向用户澄清**再给方案**。

## 必须遵守的规则

1. **隐私合规（v9.x+）**
   - 调用 SDK 任何接口前必须先征得用户同意并调用隐私接口：`LocationClient.setAgreePrivacy(true)`，且**必须在 `new LocationClient(context)` 之前**调用。见 [overview.md](references/overview.md) 隐私合规小节。

2. **AK 与配置**
   - 使用前需在控制台申请 Android SDK 密钥（AK），应用类型选「Android SDK」，填写包名、SHA1。AK 配置到 AndroidManifest 的 `<meta-data>`；包名与签名须与申请时一致。见 [overview.md](references/overview.md)。

3. **生命周期管理**
   - 在 Activity/Service 的 `onDestroy` 中必须调用 `mLocationClient.stop()`，避免定位服务持续运行消耗电量。

4. **坐标系统一**
   - 默认建议使用 `bd09ll`；若与百度地图 SDK 混用须保持坐标系一致。坐标系说明见 [location-config.md](references/location-config.md)。

5. **监听先于启动**
   - 必须先调用 `registerLocationListener` 注册监听，再调用 `start()` 启动定位；否则可能收不到回调。

6. **版本与 API 以工程为准**
   - 用户已集成 SDK 时，以其工程内版本为准。若某类或方法不存在，**提示用户将依赖更新到兼容版本**后再重试，勿强行写不存在的 API。

## 输出规范（可评估）

- **可落地**：含具体类名、方法、调用顺序与必要配置（AndroidManifest、AK、隐私、定位 Service）。
- **可验证**：隐私与 AK 明确；监听先于启动顺序正确；若涉及定位失败，方案可指向 [errorcode.md](references/errorcode.md) 或 [project-config.md](references/project-config.md) 的排查项。
- **可组合**：按 [reference.md](references/reference.md) 选文档与常见组合。

方案结构：需求 → 对应文档 → 配置与依赖 → 关键 API → 示例片段 → 注意事项。

## 参考索引

- 按需求选文档与常见组合：[reference.md](references/reference.md)
- Gradle 依赖（基础包/全量包/版本说明）：[gradle.md](references/gradle.md)
- AndroidManifest、权限、定位 Service、混淆：[project-config.md](references/project-config.md)
- 集成步骤、AK、隐私合规、LocationClient 初始化：[overview.md](references/overview.md)
- 单次/连续定位、BDLocation 字段、定位类型：[basic-location.md](references/basic-location.md)
- LocationClientOption 全参数、场景化配置：[location-config.md](references/location-config.md)
- 圆形/多边形地理围栏、进出事件监听：[geofence.md](references/geofence.md)
- 逆地理编码、地址字段、POI 周边检索：[geocode.md](references/geocode.md)
- 定位结果错误码（locType）与鉴权错误码：[errorcode.md](references/errorcode.md)
- 类与包速查：[class-reference.md](references/class-reference.md)
