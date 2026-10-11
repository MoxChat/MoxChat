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

- 更新包完成时间（UTC）：`2026-10-11T20:41:17+00:00`。
- JavaScript、品牌资源与 MLS 源码：Mox 主仓库提交 `04a4eb5a2f44d9f206eb066b5d3a8c23bf7bb7aa` 加已标识的工作树补丁。补丁 SHA-256：`27762c3f9e6b408621f3bcb4c847751fd005ed25347da818eec4575232a24b9c`；本地构建清单记录了 121 个修改、新增或删除的源码文件。未纳入无关的 iOS 工程配置与 LPK 改动。
- 打包方式：重新导出生产 Metro 资源、编译 Hermes 字节码并重建图标与启动资源；复用源码提交 `1e6ea7f6457306e9a12aa478f27397127caf6e39` 对应的原生基础包。原有 148 个原生库和 7 个 DEX 与之前 APK 逐字节一致。本次并非重新执行完整 Gradle 构建，而是加入四种架构新编译的 `libmox_mls_jni.so` 后重建并以原证书签名。
- 原生 MLS：使用当前 MoxCore Rust/C++ 源码构建，包含 SQLCipher 和静态编译的 OpenSSL。之前 APK 漏打包 JNI 库；正式入口 `MoxChat/scripts/build-android-release.mjs` 现会编译并启用 MLS，准备失败时停止，不引入 TypeScript 密码回退。
- 本次工具链：Node.js 24.18.1、现有 Expo 依赖中的 Hermes、Temurin Java 17.0.20.1（最终元数据清理/签名使用 17.0.16）、Android Build Tools 36.0.0、NDK 27.1.12297006、锁定的 Rust 依赖及 apktool 3.0.3。原生基础包使用 Gradle 8.14.3 和 CMake 3.22.1。
- 包名 `com.ponzs.moxchat`，版本 `1.3.2`，版本号 `2`，目标 SDK `36`，最低 SDK `26`（**Android 8.0+**）。包含 `arm64-v8a`、`armeabi-v7a`、`x86`、`x86_64`，内置资源，无需 Metro。
- 签名：沿用项目现有 **Android debug/test 测试密钥**，尚未配置生产分发密钥；覆盖更新要求签名一致。
- 界面修正：删除旧抽屉及旧页面分支，使用当前四栏导航与共享表单；修复发现路由、资料头像与顶部间距、搜索控件、输入栏图标与触控、深色发送图标、键盘避让、折叠屏/平板分栏及真实分区拖拽。扣除 Tab 侧栏和分隔条后，两块内容区默认等宽。
- 中继健康：由应用会话持有，进入前台立即检查，以三秒周期轮询；合并重叠检查、限制超时，手动和全局检查使用同一身份；安卓回到前台时再次检查。仍要求签名和身份认证校验成功，未绕过健康判定。
- 平台隔离：安卓输入按钮、文本输入、键盘避让、分隔手势及启动预加载逻辑使用平台适配；iOS/Web 保留原启动预加载逻辑。新增 SQLCipher OpenSSL 依赖只对 Android 生效；共享分栏修复同时处理已报告的 iPad 拖拽问题。原有未提交的 iOS 工程配置保持原样。
- 验证：APK ZIP 完整性、签名证书一致、16 KB ZIP 对齐、四种 MLS 库的 JNI 导出/ELF 对齐、原有原生库与 DEX 内容一致、TypeScript 和针对性 lint。在本次专用 Android 16 arm64 模拟器检查小屏/大屏导航、搜索、资料、系统键盘位置、反复语音/文本切换、深色图标、真实拖拽、自动中继健康和原生 MLS 启动；另用独立 iPad 模拟器加载当前 JavaScript 检查共享布局，用浏览器检查新导出的本地生产 Web 资源。此前 Find N2 真机证据对应前面的构建；最终 APK 未重复执行真机验证。这些检查不等于全平台穷尽验收或完整消息端到端验收。
- 最终元数据清理：Hermes 调试信息中的本地源码路径前缀已映射为 `/build/mox/source/export`。字节码长度及调试段之前的全部字节与模拟器测试包一致，其他 2,832 个 ZIP 条目逐字节一致；已重新生成并校验 Hermes 尾部校验值及 APK 签名。此元数据清理包在专用模拟器清理后未再次安装。
- 分发：[下载更新 APK](https://github.com/MoxChat/MoxChat/releases/download/v1.3.2/moxchat-android-1.3.2.apk)。Web ZIP 与懒猫微服 LPK 仍为上文记录的已发布版本，本次 Web 回归产物未替换它们；不含桌面安装包。
- 安装：先校验 SHA-256，再下载并打开 APK，按提示允许对应来源安装。此次替换仍使用原文件名和版本号，请通过校验值区分新旧文件。

| 产物 | SHA-256 |
| --- | --- |
| `moxchat-web-1.3.2.zip` | `1597d701da863c1a1d6a1ca24638a30e8a8632765d70bac76809513881d74020` |
| `moxchat-1.3.2.lpk` | `62fd1b52bbb522707eb1edc69cd3394e6bc8640face5f6acf291f4fe83c60070` |
| `moxchat-android-1.3.2.apk` | `3435a97c6a9d2f27bd3f3293748098c6217739bcfba28aa81984efb04a313f1d` |
