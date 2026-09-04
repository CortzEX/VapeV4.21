[简体中文](README.md) | [English](README_EN.md)

# VapeV4.21 Community Archive

Vape 4.21 Java 层与 Windows x64 原生桥接层的社区存档与研究性恢复工程。

本仓库用于保存公开的 Vape 4.21 恢复工程源码、构建脚本、原生桥接代码及相关研究成果，方便后续进行源码研究、兼容性分析、构建和维护。

> [!WARNING]
> 本项目不是 Vape 官方源码、官方发布包或厂商签名产物。
>
> 本项目与 Vape / Manthe、Lunar Client、Mojang Studios、Microsoft 均无官方关联。
>
> 请仅在你拥有并获准测试的环境中使用，并自行确认当地法律、软件许可以及服务器规则。

---

## 项目来源

本仓库保存的代码来自公开的 Vape 4.21 恢复工程。

相关项目包括：

- [OpenVapeCN/OpenVape](https://github.com/OpenVapeCN/OpenVape)
- [OpenVapeCN/VapeV4.21](https://github.com/OpenVapeCN/VapeV4.21)
- [RSSeeker/Vape-v4.21](https://github.com/RSSeeker/Vape-v4.21)

本仓库的目的主要是社区存档、源码保存、构建维护以及兼容性研究。

感谢所有参与 Vape 4.21 恢复、逆向分析、映射整理和原生桥接工作的贡献者。

---

## 项目内容

本项目包含：

- Vape 4.21 Java 层恢复代码
- Windows x64 Native Bridge
- JNI / JVMTI 相关代码
- Java Injection Payload
- Windows x64 Injector
- Minecraft Runtime Mapping
- Gradle 构建系统
- CMake 原生构建系统
- 自动化构建与校验脚本
- Native 测试代码

项目中的源码主要位于：

```text
src/main/java/
native/
build.gradle
settings.gradle
gradle/
gradlew
gradlew.bat
```

---

## Minecraft 兼容性

| Minecraft | Vanilla | Forge | Fabric |
| --- | :---: | :---: | :---: |
| 1.7.10 | ✓ | ✓ | - |
| 1.8.9 | ✓ | ✓ | - |
| 1.12.2 | ✓ | ✓ | - |
| 1.16.5 | | | |
| 1.21.11 | ✓ | ✓ | ✓ |
| 26.2 | ✓ | ✓ | ✓ |

也支持 Lunar Client 与 Badlion Client 1.8.9 实例注入。

Minecraft 1.16.5 的支持不佳，部分映射、渲染和模块功能可能无法正常工作。

**对于 26.2 版本，请在进入服务器或单人世界后注入。**

所有目标实例均须使用 64 位 JVM。

实际模块兼容性可能受到 Minecraft 版本、Loader、Mappings、渲染实现以及客户端内部实现变化的影响。

---

## 环境要求

仅编译和校验 Java 层需要：

- JDK 17，用作 Gradle toolchain；输出默认通过 `--release 8` 编译
- 项目自带的 Gradle Wrapper；构建脚本固定要求 Gradle 8.8
- 可访问 Maven Central 和 Gradle Plugin Portal 的网络连接

构建 Native Bundle 还需要：

- Windows x64
- Visual Studio 2022 C++ x64 工具链及 Windows SDK
- CMake 3.21 或更高版本
- 一套包含 JNI/JVMTI 头文件的 JDK；面向 1.7.10、1.8.9 和 1.12.2 测试时建议使用 JDK 8

---

## 快速开始

在 PowerShell 中进入仓库根目录：

```powershell
.\gradlew.bat clean build verifyInjectionPayload
```

该命令会完成以下工作：

1. 编译恢复源码并处理全部资源。
2. 检查源码数量以及残留的致命 CFR 反编译标记。
3. 生成包含运行时依赖的 injection JAR。
4. 确认载荷包含必要包，且所有 class 均可由 Java 8 加载。

主要 Java 产物位于 `build/libs/`。如需生成 IntelliJ IDEA 工程配置，可运行：

```powershell
.\gradlew.bat idea
```

## 构建原生测试包

```powershell
.\gradlew.bat prepareInjectionBundle -PtargetRelease=8 `
  -PnativeJavaHome="C:\Program Files\Java\jdk1.8.0_301"
```

完整测试包输出到 `build/injection/`：

```text
Vape421Native.dll
Vape421Injector.exe
README.md
```

DLL 将 Java injection JAR 作为 `RCDATA` 嵌入，不要求另行放置 payload。原生桥接层只实现从样本九项 `RegisterNatives` 表恢复出的接口；未在样本中注册的额外 Java native 声明不会被臆造实现。更多细节见 [`native/README.md`](native/README.md)。

---

## 常用校验任务

| 命令 | 用途 |
| --- | --- |
| `.\gradlew.bat check` | 编译、源码覆盖与恢复质量检查 |
| `.\gradlew.bat injectionJar` | 构建自包含 Java 注入载荷 |
| `.\gradlew.bat verifyInjectionPayload` | 检查依赖完整性与 Java 8 字节码版本 |
| `.\gradlew.bat buildNative` | 构建 x64 DLL 和注入器 |
| `.\gradlew.bat prepareInjectionBundle` | 汇总可供隔离测试的 native bundle |

---

## 存档说明

本仓库建立的主要目的之一是保存 Vape 4.21 恢复工程的公开源码和构建基础。

如果上游仓库发生删除、不可访问或停止维护的情况，希望本仓库能够作为社区存档，避免已有研究成果和构建基础再次丢失。

本仓库不会声称自己是原作者或官方项目，也不会以官方项目名义发布。

---

## 安全说明

本项目包含 Windows 进程注入以及 JVM Native 交互代码。

因此：

- 安全软件可能对构建产物进行检测
- 建议优先自行检查源码并自行编译
- 建议在隔离环境中进行测试
- 不要因为出现安全软件告警就直接关闭安全软件
- 不要在仓库、Issue 或日志中公开账号密码、Token、Cookie 等敏感信息

---

## License

本仓库使用 **CC0 1.0 Universal**。

具体内容请参阅 [`LICENSE`](LICENSE)。

需要注意的是，CC0 仅适用于贡献者有权进行授权的内容。

第三方库、商标、字体、纹理、游戏素材、反编译/恢复材料以及其他既有知识产权仍可能受到其各自许可证或权利人的约束。

---

## Disclaimer

本项目仅用于：

- 软件恢复研究
- 兼容性分析
- 源码研究
- 教育与技术研究
- 自有环境测试

使用者应自行确认其行为符合当地法律、软件许可证、Minecraft 服务器规则以及其他第三方服务条款。

---

## Credits

感谢参与相关恢复工作的项目与贡献者：

- [OpenVapeCN/OpenVape](https://github.com/OpenVapeCN/OpenVape)
- [OpenVapeCN/VapeV4.21](https://github.com/OpenVapeCN/VapeV4.21)
- [RSSeeker/Vape-v4.21](https://github.com/RSSeeker/Vape-v4.21)

感谢所有参与 Vape 4.21 Java 层恢复、Native Bridge、Mappings、逆向分析、测试以及兼容性修复的贡献者。

---

<p align="center">
  VapeV4.21 Community Archive
</p>
