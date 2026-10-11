# MoxChat 1.3.2 构建信息

[English](./BUILD-INFO.md)

## Web 与懒猫微服

- 源码提交：Mox 主源码仓库的 `1e6ea7f6457306e9a12aa478f27397127caf6e39`。
- Web 与 LPK 构建完成时间（UTC）：`2026-10-11T17:09:46Z`。
- 构建入口：通过现有懒猫微服发布配置执行 `MoxChat/build.sh`。
- 产物：可独立部署的 Web ZIP 压缩包和懒猫微服 LPK v2 安装包。
- Web 文件数：508；ZIP、LPK 中的内容与本次新导出的 Web 文件逐一匹配。
- 工具链：Node.js 24.18.1、pnpm 11.14.0、wasm-bindgen 0.2.126；在隔离源码副本中复用现有依赖和 Rust 缓存。
- Rust 构建：使用 `--remap-path-prefix` 将本地源码与工具链路径映射到通用 `/build` 路径，重新生成 Web 资源后打包。
- 源码状态：基于上述提交构建。主工作区未提交的 iOS 项目配置未纳入构建，且保持原样。
- 验证：生产导出、ZIP 完整性、LPK 版本与内容、必要 Worker、MLS/SQLite WASM 格式、LPK 清单中的跨源隔离配置、产物校验值，以及不含私钥内容和本地开发者路径。这些是构建与包完整性检查，未执行懒猫微服安装或浏览器功能验收。

## Android

- 基础源码提交：`1e6ea7f6457306e9a12aa478f27397127caf6e39`；隔离的 Android 构建另包含下述两行备忘录编辑器修正。原工作树保持原样。
- 构建与包验证完成时间（UTC）：`2026-10-11T18:24:51Z`。
- 构建入口：`node MoxChat/scripts/build-android-release.mjs`（`assembleRelease`）；已内置 JavaScript 资源，无需 Metro 开发服务器。
- 包名：`com.ponzs.moxchat`，版本 `1.3.2`，版本号 `2`，目标 SDK `36`。
- 安装要求：Android **8.0 或以上**（最低 SDK `26`）。包含 `arm64-v8a`、`armeabi-v7a`、`x86` 和 `x86_64` 四种架构。
- 签名：release 构建使用项目现有的 **Android debug/test 测试密钥**，并非生产分发密钥。
- 工具链：Temurin Java 17.0.20.1、Gradle 8.14.3、Android SDK 36、应用 Build Tools 36.0.0、NDK 27.1.12297006 和 CMake 3.22.1。
- 仅本次构建的兼容设置：当前 WebGPU 依赖使用 Android 26 的硬件缓冲 API，因此最低 SDK 提升为 26；三个未显式指定 NDK 版本的依赖沿用项目的 NDK 27.1。16 GB 内存构建机上的 Gradle 任务和 Ninja 编译并发均限制为二。
- Android 源码修正：`MoxNoteEditorModule.kt` 的覆盖方法及 `super` 调用使用 Android 正确的 `onSelectionChanged`，替换拼写错误的 `onNoteSelectionNoteChanged`。补丁 SHA-256：`5d75b47a231de1a82a5cefafff7b0a71a17674424db1adf47242d5ab81de0b88`。补丁与完整构建报告随本地 APK 保留；此修正仅应用于隔离的 Android 构建副本。
- 验证：release 构建及必要 lint 检查通过，APK ZIP 完整性、APK v2 签名、包名/版本/最低系统版本、四种架构、内置 JavaScript 资源、产物校验值均通过检查；未包含 keystore、环境文件或 source map，原生库不含原工作区的开发者路径。未执行 Android 真机或模拟器安装及功能验收。
- 安装：先校验文件，再将 APK 下载到 Android 8.0+ 设备，打开并按提示允许该来源安装。覆盖更新已有应用要求签名密钥一致。
- 分发：APK 超过 GitHub 仓库单文件大小限制，保存在 [Release 附件](https://github.com/MoxChat/MoxChat/releases/download/v1.3.2/moxchat-android-1.3.2.apk)，校验值记录于 `android/SHA256SUMS`。

| 产物 | SHA-256 |
| --- | --- |
| `web/moxchat-web-1.3.2.zip` | `1597d701da863c1a1d6a1ca24638a30e8a8632765d70bac76809513881d74020` |
| `lpk/moxchat-1.3.2.lpk` | `62fd1b52bbb522707eb1edc69cd3394e6bc8640face5f6acf291f4fe83c60070` |
| `moxchat-android-1.3.2.apk`（Release 附件） | `c1c5d7946854c302ae5b06cf946867c2719b0741f1a0fbcbace9f907be595723` |

各产物目录提供对应的 `SHA256SUMS`。GitHub Releases 提供安装包和压缩包的合并 `SHA256SUMS`。安装或部署前请先校验文件。
