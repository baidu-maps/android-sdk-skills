# Gradle 集成

Android 推荐通过 Gradle 依赖集成百度定位 SDK。

**版本**：以下以 9.6.x 为例，实际以官网更新为准；API 以工程内依赖版本为准，类/方法不存在时提示更新依赖，见 [SKILL.md](../SKILL.md) 规则 6。

## 1. 配置仓库

在 Project 的 `build.gradle` 或 `settings.gradle`（Gradle 7+）中添加 `mavenCentral`：

```gradle
allprojects {
    repositories {
        mavenCentral()
    }
}
```

## 2. 添加依赖（app/build.gradle）

| SDK 类型 | 依赖 | 说明 |
|----------|------|------|
| **基础定位包** | `implementation 'com.baidu.lbsyun:BaiduMapSDK_Location:9.6.6'` | 仅包含基础定位能力，包体较小 |
| **全量定位包** | `implementation 'com.baidu.lbsyun:BaiduMapSDK_Location_All:9.6.6'` | 含室内定位、地理围栏等全量能力 |

**二者互斥，只能选其一**。推荐：
- 仅需普通定位 → 基础定位包
- 需要地理围栏、室内定位 → 全量定位包

```gradle
dependencies {
    // 全量定位包（含地理围栏、室内定位）
    implementation 'com.baidu.lbsyun:BaiduMapSDK_Location_All:9.6.6'
}
```

## 3. NDK 架构（abiFilters）

```gradle
android {
    defaultConfig {
        ndk {
            abiFilters "armeabi-v7a", "arm64-v8a", "x86", "x86_64"
        }
    }
}
```

按需保留所需架构；仅上架 Google Play 时可只保留 `armeabi-v7a` 与 `arm64-v8a` 以减小包体。

## 4. 注意事项

- Gradle 依赖仅支持标准国内版；若需特殊定制包，从官网下载 jar + so，采用本地依赖方式集成（`implementation fileTree(dir: 'libs', include: ['*.jar'])` + `sourceSets.main.jniLibs.srcDirs`）。
- 集成完成后需配置 AndroidManifest（AK、权限、定位 Service）与混淆，见 [project-config.md](project-config.md)；隐私合规与初始化见 [overview.md](overview.md)。

## 5. 环境常见问题

| 问题 | 处理 |
|------|------|
| **Gradle Sync 超时** | 国内网络可将 `distributionUrl` 改为腾讯云镜像：`https://mirrors.cloud.tencent.com/gradle/gradle-8.7-bin.zip` |
| **Java 21 + Gradle 8.2 不兼容** | 升级到 Gradle 8.7+，同步升级 Android Gradle Plugin（如 8.5.2） |
| **无法找到 JDK** | macOS 下在 `~/.zshrc` 中设置 `export JAVA_HOME="/Applications/Android Studio.app/Contents/jbr/Contents/Home"` |
