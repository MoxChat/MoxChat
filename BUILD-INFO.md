# MoxChat 1.3.2 build information

[中文构建信息](./BUILD-INFO.zh-CN.md)

## Web and Lazycat

- Source revision: `1e6ea7f6457306e9a12aa478f27397127caf6e39` in the main Mox source repository.
- Build completed (UTC): `2026-10-11T17:09:46Z`.
- Build entry: `MoxChat/build.sh` via the existing Lazycat release configuration.
- Packages: independently deployable Web ZIP and Lazycat LPK v2.
- Web files: 508; the ZIP, LPK content, and fresh Web export match file for file.
- Toolchain: Node.js 24.18.1, pnpm 11.14.0, wasm-bindgen 0.2.126; existing dependencies and Rust cache reused in an isolated source checkout.
- Rust build: local source and toolchain paths remapped to generic `/build` paths using `--remap-path-prefix`; generated Web assets rebuilt before packaging.
- Source state: built from the identified commit. The uncommitted iOS project configuration in the main workspace was excluded and left untouched.
- Validation: production export, ZIP integrity, LPK version and content, required workers, MLS/SQLite WASM format, cross-origin isolation in the LPK manifest, artifact checksums, and absence of private key material and local developer paths. These are build and package checks; installation on Lazycat and browser functionality were not acceptance-tested.

## Android

- Updated package completed (UTC): `2026-10-12T14:32:39.293081+00:00`.
- Source: main Mox revision `04a4eb5a2f44d9f206eb066b5d3a8c23bf7bb7aa` plus the existing UI/MLS worktree changes and this Android-only network/menu fix. The prior UI/MLS patch SHA-256 is `27762c3f9e6b408621f3bcb4c847751fd005ed25347da818eec4575232a24b9c`; the additional five-file fix patch SHA-256 is `11790c3486ae7101a1d972a8f84df231afc24eb80c6dbe21d9eea82c691ade0c`. The retained production source-hash manifest SHA-256 is `0f36266300176f427feaf819416b621b6da7addf25db2da9ad6190e1c3defee0`.
- Build method: fresh production Metro export and Hermes bytecode from an isolated current-worktree snapshot. The previous 1.3.2 native APK (SHA-256 `3435a97c6a9d2f27bd3f3293748098c6217739bcfba28aa81984efb04a313f1d`) provides the native base, resources and four-ABI MLS libraries. The Android network helper was compiled and dexed; the Application DEX was rebuilt with its startup call, and a helper DEX was added. All shared libraries and the other six DEX files match that base byte for byte (158 unchanged entries). This is not a full Gradle native rebuild.
- Toolchain: Node.js 24.18.1, Hermes 98 from the installed Expo dependencies, Temurin Java 17.0.16+8, Android Build Tools 36.0.0 and apktool 2.12.1 for DEX assembly. The native base and MLS toolchain are unchanged from the preceding package.
- Package: `com.ponzs.moxchat`, version `1.3.2`, version code `2`, target SDK `36`, minimum SDK `26` (**Android 8.0+**). ABIs: `arm64-v8a`, `armeabi-v7a`, `x86`, `x86_64`. The embedded bundle does not require Metro.
- Signing: the existing project **Android debug/test key** is retained; a production distribution key has not been configured. Updating an installed app requires the same signing key.
- Current fixes: the Android composer menu uses an Android-specific React Native Modal surface for reliable touch dispatch. iOS keeps OverKeyboardView and Web keeps its existing Modal branch. Android networking prefers IPv4 while retaining IPv6 fallback and IPv6-only support; a two-second connection timeout allows address fallback. Read/write timeouts and authenticated health checks are preserved. Foreground health checks still run immediately and at a three-second interval.
- Included previous changes: modern four-tab navigation/forms, Discover routing, profile avatar/spacing, search, composer controls/icons and keyboard avoidance, equal-width tablet panes and native divider dragging; native MLS JNI libraries for all four ABIs.
- Physical-device evidence: OPPO Find N2 (PGU110), Android 16. On this device the real relay responded over IPv4 in approximately 1.36 seconds, while IPv6 timed out. The instrumented network fix recorded 1,166 HTTP 200 responses and no captured HTTP errors; an existing failed message automatically reached sent/durable state. This verifies recovery of an existing message, not a freshly composed first send. No evidence established react-native-runtimes incompatibility.
- Menu verification: with the keyboard closed, File opened the system picker and Cancel returned to chat. With the keyboard open, Album triggered the system photo permission prompt; permission was declined, so photo-grid selection was not exercised. The clean APK closed the menu with hardware Back and cold-started to a green relay indicator with `1/1` without opening the relay page. App data was preserved during installation.
- Compatibility/build checks: focused lint, Android production bundle, iOS/Web JS bundles and platform resolution, APK integrity, same signing certificate and 16-KiB ZIP alignment. iOS/Web bundles exclude the Android overlay; the existing iOS project configuration was preserved. These compilation checks do not establish complete iOS/Web device/browser regression of the earlier shared changes. Prior emulator/iPad/browser checks apply to the preceding package.
- Release metadata cleanup: a same-length local source-path prefix in Hermes debug metadata was replaced with a generic build path. Every byte before the debug section and all other APK entries remain identical to the physical-device-tested clean package. The Hermes footer checksum and APK signatures were regenerated and verified. No executable code changed; this metadata-only release package was not reinstalled. Diagnostic collection and native network logging are absent.
- Remaining verification: freshly composed first-message delivery after cold launch, photo-grid selection, camera, voice/video calls, complete end-to-end group messaging and exhaustive cross-platform regression were not acceptance-tested in this update.
- Distribution: [download the updated APK](https://github.com/MoxChat/MoxChat/releases/download/v1.3.2/moxchat-android-1.3.2.apk). Web ZIP and Lazycat LPK remain the published packages described above. Desktop packages are not included.
- Installation: verify SHA-256, download/open the APK, and allow installation from that source. This replacement keeps the filename and app version; compare the checksum to distinguish the updated file.

| Artifact | SHA-256 |
| --- | --- |
| `moxchat-web-1.3.2.zip` | `1597d701da863c1a1d6a1ca24638a30e8a8632765d70bac76809513881d74020` |
| `moxchat-1.3.2.lpk` | `62fd1b52bbb522707eb1edc69cd3394e6bc8640face5f6acf291f4fe83c60070` |
| `moxchat-android-1.3.2.apk` | `d3400a97a1933d68cbba55bffb64c7ecc98e2657d775e555ece3265339551e8f` |
