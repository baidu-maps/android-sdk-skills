# 地理围栏

## 通用规则

- **先注册广播，再添加围栏**：调用 `addGeoFence` 前须先通过 `registerGeoFenceListener` 注册 PendingIntent，否则触发时无接收方。
- 同一应用最多支持 **100 个**地理围栏。
- 圆形围栏建议半径不低于 **200 米**，过小时因定位精度误差易产生误触发。
- 围栏触发依赖定位服务持续运行，建议配合**前台 Service** 保活，见 [project-config.md](project-config.md)。

---

## 初始化与触发类型

```java
BDGeofenceClient mGeofenceClient = new BDGeofenceClient(getApplicationContext());

// 设置触发类型（可按位组合）
mGeofenceClient.setActivateAction(
    BDGeofenceClient.GEOFENCE_IN   |   // 进入围栏
    BDGeofenceClient.GEOFENCE_OUT  |   // 离开围栏
    BDGeofenceClient.GEOFENCE_STAY     // 在围栏内停留（默认阈值约 5 分钟）
);

// 注册触发广播
Intent intent = new Intent(this, GeofenceReceiver.class);
PendingIntent pendingIntent = PendingIntent.getBroadcast(
    this, 0, intent,
    PendingIntent.FLAG_UPDATE_CURRENT | PendingIntent.FLAG_MUTABLE);
mGeofenceClient.registerGeoFenceListener(pendingIntent);
```

---

## 添加圆形围栏

```java
GeoFenceOption option = new GeoFenceOption()
    .setCoordType(CoordType.BD09LL)          // 坐标系
    .setCustomId("fence_001")                // 自定义业务 ID，回调时原样返回
    .setCenterPoint(new LatLng(39.9152, 116.4039)) // 中心点（BD09LL）
    .setRadius(500f);                        // 半径，单位：米

mGeofenceClient.addGeoFence(option);
```

---

## 添加多边形围栏

```java
List<LatLng> points = new ArrayList<>();
points.add(new LatLng(39.920, 116.400));
points.add(new LatLng(39.920, 116.410));
points.add(new LatLng(39.910, 116.410));
points.add(new LatLng(39.910, 116.400));  // 至少 3 个顶点

GeoFenceOption option = new GeoFenceOption()
    .setCoordType(CoordType.BD09LL)
    .setCustomId("fence_polygon_001")
    .setPointList(points);

mGeofenceClient.addGeoFence(option);
```

---

## 围栏添加回调（可选）

```java
mGeofenceClient.setGeoFenceListener(new BDGeofenceClient.GeoFenceAddListener() {
    @Override
    public void onGeoFenceAdded(List<GeoFence> geoFenceList, int errorCode) {
        if (errorCode == GeoFence.ADDGEOFENCE_SUCCESS) {
            for (GeoFence fence : geoFenceList) {
                Log.d("Geofence", "添加成功: " + fence.getFenceId());
            }
        } else {
            Log.e("Geofence", "添加失败，errorCode: " + errorCode);
        }
    }
});
```

---

## 广播接收器

```java
public class GeofenceReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        if (!BDGeofenceClient.ACTION_RECEIVER_FENCE.equals(intent.getAction())) return;

        int    actionType = intent.getIntExtra(BDGeofenceClient.GEOFENCE_STATUS, -1);
        String customId   = intent.getStringExtra(BDGeofenceClient.GEOFENCE_CUSTOM_ID);
        double lat        = intent.getDoubleExtra(BDGeofenceClient.GEOFENCE_LATITUDE, 0);
        double lng        = intent.getDoubleExtra(BDGeofenceClient.GEOFENCE_LONGITUDE, 0);

        switch (actionType) {
            case BDGeofenceClient.GEOFENCE_IN:
                // 进入围栏
                break;
            case BDGeofenceClient.GEOFENCE_OUT:
                // 离开围栏
                break;
            case BDGeofenceClient.GEOFENCE_STAY:
                // 停留在围栏内
                break;
        }
    }
}
```

在 AndroidManifest 中注册（显式广播，避免 Android 8.0+ 隐式广播限制）：

```xml
<receiver
    android:name=".GeofenceReceiver"
    android:exported="false">
    <intent-filter>
        <action android:name="com.baidu.geofence.GeoFenceReceiver" />
    </intent-filter>
</receiver>
```

---

## 围栏管理

```java
mGeofenceClient.removeGeoFence("fence_001");  // 移除指定围栏（传 customId）
mGeofenceClient.removeGeoFence(null);          // 移除所有围栏
mGeofenceClient.pauseGeoFence();               // 暂停监听
mGeofenceClient.resumeGeoFence();              // 恢复监听
```
