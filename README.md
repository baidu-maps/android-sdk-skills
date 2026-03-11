# 百度地图 Android Skills

本仓库提供一组面向 **Android 百度地图** 平台的 AI Coding Agent Skills，帮助开发者在智能研发工具（如 Cursor / Claude Code 等）中高效完成 Android 应用的创建、地图 SDK 集成与 Java/Kotlin 代码开发。

## Skills 概览

本仓库包含以下三个 Skill：

| Skill | 说明 |
| --- | --- |
| [baidu-map-android-sdk](skills/baidu-map-android-sdk/) | 百度地图 Android SDK 开发助手 |
| [android-build](skills/android-build/) | Android 工程编译助手 |
| [android-empty-project](skills/android-empty-project/) | Android 空工程模板生成器 |

---

### baidu-map-android-sdk

百度地图 Android SDK 的完整开发辅助 Skill，覆盖以下核心能力：

- **地图展示与交互** — MapView 生命周期、手势控件、地图类型切换、个性化地图、室内图、离线地图等
- **覆盖物绘制** — Marker、折线、多边形、圆、气泡信息窗、点聚合、3D 模型、轨迹动画等
- **检索能力** — POI/AOI 检索、地理编码/逆地理编码、建筑物/行政区/天气/公交线路检索、推荐上车点等
- **路线规划** — 驾车、步行、骑行、公交、跨城公交路线规划
- **步骑行导航** — 导航引擎初始化、路线规划、导航控制与状态监听、默认导航 UI、模拟导航、语音播报
- **定位能力** — 前台/后台连续定位、单次定位、经纬度/地址/POI 获取
- **工具类** — 距离/面积计算、坐标转换、空间关系判断、调起百度地图客户端

支持独立包（BaiduLBS_Android、定位 SDK）和组合包，以及步骑行导航引擎。

### android-build

Android 工程编译助手，通过 Gradle 命令行（`gradlew`）编译 Android 工程，适用于：

- 本地构建与调试
- 自动化脚本构建
- CI/CD 持续集成/持续部署

提供 Debug/Release APK 打包、AAB 构建、常见构建问题排查等能力。

### android-empty-project

Android 空工程模板生成器，可从零创建标准的 Android 最小可运行工程，包括：

- 完整的工程目录结构（app 模块、src/main 等）
- 标准配置文件（settings.gradle.kts、build.gradle.kts、gradle.properties 等）
- 入口 Activity（MainActivity）
- 资源文件（strings.xml、themes.xml）
- Gradle Wrapper 配置
- 支持 Kotlin/Java 语言
- 支持 Gradle Kotlin DSL（推荐）或 Groovy

---

## 适用环境

- **平台**：Android 5.0（API 21）及以上
- **开发工具**：Android Studio Arctic Fox 及以上
- **Gradle 版本**：7.0 及以上（推荐 8.0+）
- **JDK 版本**：JDK 11 或 JDK 17（与项目 compileSdk/AGP 匹配）
- **语言**：Kotlin（推荐）或 Java

## 目录结构

```
android-sdk-skills/
├── README.md                 # 本文件（中文）
├── README_EN.md              # English README（待生成）
├── CHANGELOG.md              # 变更日志（待生成）
└── skills/
    ├── baidu-map-android-sdk/
    │   ├── SKILL.md          # Skill 定义文件
    │   └── references/       # API 文档、开发指南、资源文件
    ├── android-build/
    │   ├── SKILL.md          # Skill 定义文件
    │   └── reference.md      # 参考文档
    └── android-empty-project/
        ├── SKILL.md          # Skill 定义文件
        └── reference.md      # 参考文档
```

## 使用方式

### 1. 克隆本仓库

```bash
git clone <本仓库地址>
cd android-sdk-skills
```

### 2. 从 Release 下载（可选）

你也可以直接从 Release 下载附件 `android-sdk-skills.zip`，然后解压使用：

```bash
unzip android-sdk-skills.zip
cd skills
```

### 3. 将 Skill 注册到你的 AI 助手

把 `skills/` 目录下的 `baidu-map-android-sdk`、`android-build`、`android-empty-project` 链接或复制到当前环境对应的 skills 目录，这样 AI 在对话时会自动读取这些文档。

**Claude Code（本地）**

- Skills 目录一般为：`~/.claude/skills/`
- 注册（软链，推荐）：
  ```bash
  ln -sfn "$(pwd)/skills/baidu-map-android-sdk" ~/.claude/skills/baidu-map-android-sdk
  ln -sfn "$(pwd)/skills/android-build" ~/.claude/skills/android-build
  ln -sfn "$(pwd)/skills/android-empty-project" ~/.claude/skills/android-empty-project
  ```
- 或直接把 `skills/` 下的文件夹复制到 `~/.claude/skills/` 下。

**Cursor**

- Skills 目录一般为：`~/.cursor/skills-cursor/`
- 注册（软链，推荐）：
  ```bash
  ln -sfn "$(pwd)/skills/baidu-map-android-sdk" ~/.cursor/skills-cursor/baidu-map-android-sdk
  ln -sfn "$(pwd)/skills/android-build" ~/.cursor/skills-cursor/android-build
  ln -sfn "$(pwd)/skills/android-empty-project" ~/.cursor-cursor/skills/android-empty-project
  ```
- 或直接把 `skills/` 下的文件夹复制到 `~/.cursor/skills-cursor/` 下。

### 4. 在对话中使用

在支持 Skills 的客户端里，当你的问题涉及「百度地图」「Android 地图 SDK」「MapView」「BaiduMap」「Android 编译」「Gradle 构建」「Android 空工程」等关键词时，助手会优先参考本仓库中对应 Skill 的文档来回答，从而给出更贴合百度地图 Android SDK 的代码与用法。

## 变更日志

详见 [CHANGELOG.md](CHANGELOG.md)。

## 许可证

本项目为百度内部项目，仅供授权使用。
