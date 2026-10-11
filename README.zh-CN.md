# MoxChat

[English](./README.md)

本仓库专门用于分发 MoxChat 的 iOS、Android、macOS、Linux 和 Windows 客户端安装包，以及可独立部署的 Web 包和懒猫微服 LPK 包。应用源码和构建工具维护在 Mox 主源码仓库中。

- iOS：[在 App Store 下载](https://apps.apple.com/us/app/moxchat/id6775016915)
- Web：[打开 MoxChat 网页版](https://app.ponzs.com)

| 平台 / 产物 | 用途 | 分发位置 |
| --- | --- | --- |
| iOS | 通过苹果 App Store 安装 iOS 客户端，本仓库提供商店下载入口。 | [App Store](https://apps.apple.com/us/app/moxchat/id6775016915) |
| Android | 面向 Android 设备的 APK 安装包。 | [`android/`](./android/) |
| macOS | 面向 Mac 电脑的桌面客户端安装包或应用压缩包。 | [`macos/`](./macos/) |
| Linux | 面向 Linux 电脑的桌面客户端安装包或便携压缩包。 | [`linux/`](./linux/) |
| Windows | 面向 Windows 电脑的桌面客户端安装程序或便携包。 | [`windows/`](./windows/) |
| Web | 已构建的 Web 客户端资源，可在自己的主机上独立部署并通过浏览器访问；也可以直接使用上方链接中的在线版本。 | [`web/`](./web/) |
| 懒猫微服 LPK | 将 MoxChat Web 客户端打包为 LPK，用于在懒猫微服上安装和部署。 | [`lpk/`](./lpk/) |

**1.3.2** 现已提供：[Web ZIP 压缩包](https://github.com/MoxChat/MoxChat/releases/download/v1.3.2/moxchat-web-1.3.2.zip)、[懒猫微服 LPK 安装包](https://github.com/MoxChat/MoxChat/releases/download/v1.3.2/moxchat-1.3.2.lpk)和[Android APK](https://github.com/MoxChat/MoxChat/releases/download/v1.3.2/moxchat-android-1.3.2.apk)。请查看[发布说明和校验文件](https://github.com/MoxChat/MoxChat/releases/tag/v1.3.2)及[构建信息](./BUILD-INFO.zh-CN.md)。macOS、Linux 和 Windows 安装包暂未发布。Android APK 要求 Android 8.0+，包含四种架构，为使用现有 debug/test 测试密钥签名的 release 构建。安装前请先校验文件，再在设备上打开 APK，并按提示允许该来源安装。

独立部署 Web 时，先校验文件，再将 ZIP 解压到网站根目录，保留全部文件和路径。网站需使用 HTTPS，并在响应中设置 `Cross-Origin-Opener-Policy: same-origin` 和 `Cross-Origin-Embedder-Policy: require-corp`。请以 `application/wasm` 类型提供 `.wasm` 文件，并允许直接访问导出的 HTML 路由。ZIP 包含已构建的客户端，托管服务器无需安装 Node.js。部署后在浏览器中打开网站，确认应用正常加载，没有资源缺失或跨源隔离错误。

懒猫微服用户请先校验文件，再通过应用界面安装 LPK，或运行 `lzc-cli app install moxchat-1.3.2.lpk`。要求懒猫微服系统 1.5.0 或以上。LPK 已包含 Web 客户端和浏览器隔离响应头配置，安装后打开分配的 `moxchat` 地址即可。
