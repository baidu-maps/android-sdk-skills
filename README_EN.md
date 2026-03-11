# Baidu Map Android Skills

This repository provides a set of AI Coding Agent Skills for the **Android Baidu Map** platform, helping developers efficiently create Android applications, integrate the Baidu Map SDK, and develop Java/Kotlin code within intelligent development tools (such as Cursor / Claude Code).

## Skills Overview

This repository contains the following three Skills:

| Skill | Description |
| --- | --- |
| [baidu-map-android-sdk](skills/baidu-map-android-sdk/) | Baidu Map Android SDK Development Assistant |
| [android-build](skills/android-build/) | Android Project Build Assistant |
| [android-empty-project](skills/android-empty-project/) | Android Empty Project Template Generator |

---

### baidu-map-android-sdk

A comprehensive development assistant Skill for the Baidu Map Android SDK, covering the following core capabilities:

- **Map Display & Interaction** — MapView lifecycle, gesture controls, map type switching, custom styles, indoor maps, offline maps, etc.
- **Overlay Drawing** — Markers, polylines, polygons, circles, info windows, marker clustering, 3D models, track animations, etc.
- **Search Capabilities** — POI/AOI search, geocoding/reverse geocoding, building/district/weather/bus line search, recommended pickup points, etc.
- **Route Planning** — Driving, walking, cycling, transit, and intercity transit route planning
- **Walk & Ride Navigation** — Navigation engine initialization, route planning, navigation control & status monitoring, default navigation UI, simulated navigation, TTS voice guidance
- **Location Services** — Foreground/background continuous positioning, single positioning, latitude/longitude/address/POI retrieval
- **Utilities** — Distance/area calculation, coordinate conversion, spatial relationship analysis, launching Baidu Maps client

Supports standalone packages (BaiduLBS_Android, Location SDK) and combo packages, as well as the walk & ride navigation engine.

### android-build

An Android project build assistant that compiles Android projects via Gradle command line (`gradlew`), suitable for:

- Local build and debugging
- Automated script building
- CI/CD (Continuous Integration/Continuous Deployment)

Provides capabilities for Debug/Release APK packaging, AAB builds, and common build issue troubleshooting.

### android-empty-project

An Android empty project template generator that creates a standard Android minimal runnable project from scratch, including:

- Complete project directory structure (app module, src/main, etc.)
- Standard configuration files (settings.gradle.kts, build.gradle.kts, gradle.properties, etc.)
- Entry Activity (MainActivity)
- Resource files (strings.xml, themes.xml)
- Gradle Wrapper configuration
- Supports Kotlin/Java language
- Supports Gradle Kotlin DSL (recommended) or Groovy

---

## Requirements

- **Platform**: Android 5.0 (API 21) or above
- **IDE**: Android Studio Arctic Fox or above
- **Gradle Version**: 7.0 or above (8.0+ recommended)
- **JDK Version**: JDK 11 or JDK 17 (matching project compileSdk/AGP)
- **Language**: Kotlin (recommended) or Java

## Directory Structure

```
android-skills/
├── README.md                 # Chinese README
├── README_EN.md              # This file (English)
├── CHANGELOG.md              # Changelog
└── skills/
    ├── baidu-map-android-sdk/
    │   ├── SKILL.md          # Skill definition file
    │   └── references/       # API docs, guidelines, and assets
    ├── android-build/
    │   ├── SKILL.md          # Skill definition file
    │   └── reference.md      # Reference document
    └── android-empty-project/
        ├── SKILL.md          # Skill definition file
        └── reference.md      # Reference document
```

## Usage

### 1. Clone This Repository

```bash
git clone <repository-url>
cd android-skills
```

### 2. Download from Release (Optional)

You can also download the `android-skills.zip` attachment from the Release page, then extract and use it:

```bash
unzip android-skills.zip
cd skills
```

### 3. Register Skills with Your AI Assistant

Link or copy the `baidu-map-android-sdk`, `android-build`, and `android-empty-project` directories under `skills/` to your AI assistant's skills directory. The AI will then automatically read these documents during conversations.

**Claude Code (Local)**

- Skills directory is typically: `~/.claude/skills/`
- Register via symlink (recommended):
  ```bash
  ln -sfn "$(pwd)/skills/baidu-map-android-sdk" ~/.claude/skills/baidu-map-android-sdk
  ln -sfn "$(pwd)/skills/android-build" ~/.claude/skills/android-build
  ln -sfn "$(pwd)/skills/android-empty-project" ~/.claude/skills/android-empty-project
  ```
- Or directly copy the folders under `skills/` to `~/.claude/skills/`.

**Cursor**

- Skills directory is typically: `~/.cursor/skills/`
- Register via symlink (recommended):
  ```bash
  ln -sfn "$(pwd)/skills/baidu-map-android-sdk" ~/.cursor/skills/baidu-map-android-sdk
  ln -sfn "$(pwd)/skills/android-build" ~/.cursor/skills/android-build
  ln -sfn "$(pwd)/skills/android-empty-project" ~/.cursor/skills/android-empty-project
  ```
- Or directly copy the folders under `skills/` to `~/.cursor/skills/`.

### 4. Use in Conversations

In clients that support Skills, when your questions involve keywords like "Baidu Map", "Android Map SDK", "MapView", "BaiduMap", "Android build", "Gradle build", "Android empty project", etc., the assistant will prioritize the corresponding Skill documents from this repository, providing code and usage guidance tailored to the Baidu Map Android SDK.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for details.

## License

This is a Baidu internal project, authorized use only.
