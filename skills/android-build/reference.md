# Android 编译 - 参考

## Gradle 常用参数

| 参数 | 说明 |
|------|------|
| `--no-daemon` | 不启动守护进程（CI 常用） |
| `--no-build-cache` | 禁用构建缓存 |
| `--stacktrace` | 失败时打印完整堆栈 |
| `-p <path>` | 指定项目目录 |
| `-x test` | 跳过测试（如 `build -x test`） |

## 产物位置

- **APK**：`app/build/outputs/apk/debug/` 或 `.../release/`
- **AAB**：`app/build/outputs/bundle/release/`
- **Mapping（Release 混淆）**：`app/build/outputs/mapping/release/`

## Release 签名（简要）

在 `app/build.gradle.kts` 中配置：

```kotlin
android {
    signingConfigs {
        create("release") {
            storeFile = file("your.keystore")
            storePassword = "***"
            keyAlias = "your-alias"
            keyPassword = "***"
        }
    }
    buildTypes {
        release {
            signingConfig = signingConfigs.getByName("release")
            // ...
        }
    }
}
```

密码等敏感信息建议用环境变量或 keystore 属性文件，不要提交到仓库。

## 与 android-empty-project 的关系

- **android-empty-project**：从零创建空工程结构
- **android-build**：对已有工程执行编译、打包

---

## JDK 版本配置

AGP（Android Gradle Plugin）8.x 需要 JDK 11 或更高版本，推荐 JDK 17。

### 方式一：命令行临时指定

```bash
# 使用 JDK 17 编译
JAVA_HOME=/path/to/jdk-17 ./gradlew assembleDebug

# Windows
set JAVA_HOME=C:\path\to\jdk-17
gradlew.bat assembleDebug
```

### 方式二：gradle.properties 持久化配置

在 `gradle.properties` 中添加：

```properties
# macOS/Linux
org.gradle.java.home=/Users/username/Library/Java/JavaVirtualMachines/jbr-17.0.14/Contents/Home

# Windows
org.gradle.java.home=C\:\\Program Files\\JetBrains\\jbr-17.0.14
```

### 方式三：环境变量

```bash
export JAVA_HOME=/path/to/jdk-17
export PATH=$JAVA_HOME/bin:$PATH

# 验证
./gradlew -version
```

### 检查当前 JDK

```bash
# 查看 Gradle 使用的 JDK
./gradlew -version

# 查看 Java 版本
java -version

# 查看可用的 JDK
/usr/libexec/java_home -V  # macOS
update-alternatives --list java  # Linux
```

---

## 常见编译错误

### SDK location not found

```
SDK location not found. Define a valid SDK location with an ANDROID_HOME environment variable or by setting the sdk.dir path in your project's local properties file.
```

**解决方案：** 创建 `local.properties` 文件：
```properties
sdk.dir=/Users/username/Library/Android/sdk
```

### JDK 版本不兼容

```
Incompatible because this component declares a component for use during compile-time, compatible with Java 11 and the consumer needed a component for use during runtime, compatible with Java 8
```

**解决方案：** 使用 JDK 11 或更高版本（参见上方 JDK 版本配置）。
