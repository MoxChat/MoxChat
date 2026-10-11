# MoxChat

[中文文档](./README.zh-CN.md)

This repository is dedicated to distributing MoxChat client installation packages for iOS, Android, macOS, Linux, and Windows, as well as independently deployable Web packages and Lazycat LPK packages. Application source code and build tools are maintained in the main Mox source repository.

- iOS: [Download on the App Store](https://apps.apple.com/us/app/moxchat/id6775016915)
- Web: [Open MoxChat](https://app.ponzs.com)

| Platform / package | Purpose | Distribution location |
| --- | --- | --- |
| iOS | Install the iOS client through Apple's App Store. This repository provides the store link. | [App Store](https://apps.apple.com/us/app/moxchat/id6775016915) |
| Android | APK installation packages for Android devices. | [`android/`](./android/) |
| macOS | Desktop client installation packages or application archives for Mac computers. | [`macos/`](./macos/) |
| Linux | Desktop client installation packages or portable archives for Linux computers. | [`linux/`](./linux/) |
| Windows | Desktop client installers or portable packages for Windows computers. | [`windows/`](./windows/) |
| Web | Built Web client assets that can be deployed independently on your own host and accessed through a browser. You can also use the hosted online version linked above. | [`web/`](./web/) |
| Lazycat LPK | The MoxChat Web client packaged for installation and deployment on Lazycat MicroServer. | [`lpk/`](./lpk/) |

Version **1.3.2** is available: [Web ZIP](https://github.com/MoxChat/MoxChat/releases/download/v1.3.2/moxchat-web-1.3.2.zip) and [Lazycat LPK](https://github.com/MoxChat/MoxChat/releases/download/v1.3.2/moxchat-1.3.2.lpk). See [release notes and checksums](https://github.com/MoxChat/MoxChat/releases/tag/v1.3.2) and [build information](./BUILD-INFO.md). Android, macOS, Linux, and Windows packages have not been published yet.

For an independent Web deployment, verify the checksum and extract the ZIP into your site's document root, preserving all files and paths. Serve it over HTTPS and set `Cross-Origin-Opener-Policy: same-origin` and `Cross-Origin-Embedder-Policy: require-corp` on responses. Serve `.wasm` files as `application/wasm`; allow direct access to the exported HTML routes. The ZIP contains the built client and does not require Node.js on the hosting server. After deployment, open the site in a browser and confirm the application loads without missing assets or cross-origin isolation errors.

For Lazycat MicroServer, verify and install the LPK through the app UI or run `lzc-cli app install moxchat-1.3.2.lpk`. Lazycat OS 1.5.0 or later is required. The LPK includes the Web client and its browser isolation header configuration; open the assigned `moxchat` address after installation.
