# 错误码对照表

## 定位类型（locType）

`BDLocation.getLocType()` 返回本次定位的类型或失败原因。

| locType | 含义 |
|---------|------|
| 61 | GPS 定位成功 |
| 62 | 室内定位成功 |
| 65 | 定位缓存（上次成功位置） |
| 66 | 离线定位（无网络时使用缓存） |
| 67 | 网络定位成功（基站） |
| 68 | 网络定位成功（Wi-Fi） |
| 161 | 网络融合定位成功 |
| 162 | **定位失败**（通用失败，见下方排查） |
| 167 | 服务端定位失败 |
| 505 | AK 鉴权失败 |

**locType=162 排查步骤**：
1. 确认 `ACCESS_FINE_LOCATION` 权限已授予（运行时）。
2. 确认网络可用（网络定位需联网）。
3. 确认 AK 正确、包名与控制台一致、SHA1 与当前签名匹配。
4. 查 Logcat 关键字 `BaiduLocation` 获取详细错误信息。

---

## AK 鉴权错误码

定位 SDK 鉴权失败时，`BDLocation.getLocType()` 返回 **505**，可通过 `BDLocation.getLocationDescribe()` 获取错误信息。

| 错误码 | 含义 |
|--------|------|
| 101 | AK 参数不存在 |
| 200 | APP 不存在，AK 有误 |
| 201 | APP 被用户禁用 |
| 202 | APP 被管理员删除 |
| 203 | APP 类型错误（须选 Android SDK 类型） |
| 210 | APP IP 校验失败 |
| 230 | APP Mcode 校验失败（包名/签名不一致） |
| 240 | APP 服务被禁用（控制台未勾选定位服务） |
| 301 | 永久配额超限 |
| 302 | 天配额超限 |

**常见鉴权失败原因**：包名与控制台 applicationId 不一致；SHA1 与当前签名不一致（换 keystore 后须更新控制台）；控制台未勾选「Android SDK」类型或定位相关服务。

---

## 地理围栏错误码

`GeoFenceAddListener.onGeoFenceAdded` 中的 errorCode：

| 值 | 含义 |
|----|------|
| `GeoFence.ADDGEOFENCE_SUCCESS` (0) | 添加成功 |
| 非 0 | 添加失败，检查参数是否合法（围栏数是否超 100 个、半径是否为正数） |
