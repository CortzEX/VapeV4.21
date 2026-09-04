[English](README_EN.md) | [简体中文](README.md)

# VapeV4.21 Community Archive

A community archive and research-oriented recovery project for the Vape 4.21 Java layer and Windows x64 native bridge layer.

This repository preserves publicly available Vape 4.21 recovery-project source code, build scripts, native bridge code, and related research work for source study, compatibility analysis, building, and maintenance.

> [!WARNING]
> This project is NOT official Vape source code, an official release package, or a vendor-signed artifact.
>
> This project is not officially affiliated with Vape / Manthe, Lunar Client, Mojang Studios, or Microsoft.
>
> Use this project only in environments that you own and are authorized to test, and verify applicable laws, software licenses, and server rules yourself.

---

## Project Lineage

The code preserved in this repository comes from publicly available Vape 4.21 recovery projects.

Related projects include:

- [OpenVapeCN/OpenVape](https://github.com/OpenVapeCN/OpenVape)
- [OpenVapeCN/VapeV4.21](https://github.com/OpenVapeCN/VapeV4.21)
- [RSSeeker/Vape-v4.21](https://github.com/RSSeeker/Vape-v4.21)

The primary purpose of this repository is community preservation, source-code archival, build maintenance, and compatibility research.

Thanks to everyone who contributed to Vape 4.21 recovery, reverse analysis, mapping work, and native bridge research.

---

## Project Contents

This project contains:

- Recovered Vape 4.21 Java-layer source code
- Windows x64 Native Bridge
- JNI / JVMTI related code
- Java Injection Payload
- Windows x64 Injector
- Minecraft runtime mappings
- Gradle build system
- CMake native build system
- Automated build and verification scripts
- Native test code

The main source locations include:

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

## Minecraft Compatibility

| Minecraft | Vanilla | Forge | Fabric |
| --- | :---: | :---: | :---: |
| 1.7.10 | ✓ | ✓ | - |
| 1.8.9 | ✓ | ✓ | - |
| 1.12.2 | ✓ | ✓ | - |
| 1.16.5 | | | |
| 1.21.11 | ✓ | ✓ | ✓ |
| 26.2 | ✓ | ✓ | ✓ |

Injection into Lunar Client and Badlion Client 1.8.9 instances is also supported.

Minecraft 1.16.5 support is poor; some mappings, rendering, and module functionality may not work correctly.

**For version 26.2, inject after joining a server or singleplayer world.**

All target instances must use a 64-bit JVM.

Actual module compatibility may depend on the Minecraft version, loader, mappings, rendering implementation, and client-internal changes.

---

## Requirements

Only the following are required to compile and verify the Java layer:

- JDK 17, used as the Gradle toolchain; output is compiled with `--release 8` by default
- The Gradle Wrapper included with the project; the build script requires Gradle 8.8
- Network access to Maven Central and the Gradle Plugin Portal

Building the native bundle additionally requires:

- Windows x64
- Visual Studio 2022 C++ x64 toolchain and Windows SDK
- CMake 3.21 or newer
- A JDK containing JNI/JVMTI headers; JDK 8 is recommended when testing against 1.7.10, 1.8.9, and 1.12.2

---

## Quick Start

In PowerShell, navigate to the repository root:

```powershell
.\gradlew.bat clean build verifyInjectionPayload
```

This performs the following tasks:

1. Compiles the recovered source and processes all resources.
2. Checks source count and remaining fatal CFR decompilation markers.
3. Generates the injection JAR containing runtime dependencies.
4. Confirms that the payload contains required packages and that all classes can be loaded by Java 8.

Main Java artifacts are located in `build/libs/`. To generate IntelliJ IDEA project configuration, run:

```powershell
.\gradlew.bat idea
```

## Build the Native Test Bundle

```powershell
.\gradlew.bat prepareInjectionBundle -PtargetRelease=8 `
  -PnativeJavaHome="C:\Program Files\Java\jdk1.8.0_301"
```

The complete test bundle is written to `build/injection/`:

```text
Vape421Native.dll
Vape421Injector.exe
README.md
```

The DLL embeds the Java injection JAR as an `RCDATA` resource, so no separate payload file is required. The native bridge implements only the interfaces recovered from the sample's nine-entry `RegisterNatives` table; additional Java native declarations that were not registered in the sample are not fabricated. See [`native/README.md`](native/README.md) for more details.

---

## Common Verification Tasks

| Command | Purpose |
| --- | --- |
| `.\gradlew.bat check` | Compile, source-coverage, and recovery-quality checks |
| `.\gradlew.bat injectionJar` | Build the self-contained Java injection payload |
| `.\gradlew.bat verifyInjectionPayload` | Verify dependency integrity and Java 8 bytecode version |
| `.\gradlew.bat buildNative` | Build the x64 DLL and injector |
| `.\gradlew.bat prepareInjectionBundle` | Assemble the native bundle for isolated testing |

---

## Archive Purpose

One of the primary goals of this repository is to preserve the publicly available source code and build foundation of the Vape 4.21 recovery work.

If upstream repositories are removed, become unavailable, or stop being maintained, this repository is intended to serve as a community archive so that existing research and build foundations are not lost again.

This repository does not claim to be the original author's or an official project, and it will not present itself as an official release.

---

## Security

This project contains Windows process-injection and JVM native-interaction code.

Therefore:

- Security software may flag built artifacts
- Reviewing the source and building binaries yourself is recommended
- Testing in an isolated environment is recommended
- Do not disable security software merely because a detection occurs
- Do not publish account passwords, tokens, cookies, or other sensitive information in the repository, issues, or logs

---

## License

This repository uses **CC0 1.0 Universal**.

See [`LICENSE`](LICENSE) for the full text.

CC0 applies only to material that contributors have the legal right to dedicate.

Third-party libraries, trademarks, fonts, textures, game assets, decompiled/recovered materials, and other existing intellectual property may remain subject to their respective licenses or rights holders.

---

## Disclaimer

This project is intended for:

- Software recovery research
- Compatibility analysis
- Source-code research
- Educational and technical research
- Testing in self-owned environments

Users are responsible for ensuring that their use complies with applicable laws, software licenses, Minecraft server rules, and other third-party terms of service.

---

## Credits

Thanks to the projects and contributors involved in the related recovery work:

- [OpenVapeCN/OpenVape](https://github.com/OpenVapeCN/OpenVape)
- [OpenVapeCN/VapeV4.21](https://github.com/OpenVapeCN/VapeV4.21)
- [RSSeeker/Vape-v4.21](https://github.com/RSSeeker/Vape-v4.21)

Thanks to everyone who contributed to the Vape 4.21 Java-layer recovery, Native Bridge, mappings, reverse analysis, testing, and compatibility fixes.

---

<p align="center">
  VapeV4.21 Community Archive
</p>
