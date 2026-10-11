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

- Updated package completed (UTC): `2026-10-11T20:41:17+00:00`.
- JavaScript, branding and MLS source: main Mox revision `04a4eb5a2f44d9f206eb066b5d3a8c23bf7bb7aa` plus the identified working-tree patch. Patch SHA-256: `27762c3f9e6b408621f3bcb4c847751fd005ed25347da818eec4575232a24b9c`; 121 changed/new/deleted source files are recorded in the retained local build manifest. The unrelated iOS project configuration and LPK changes were excluded.
- Packaging: fresh production Metro export and Hermes bytecode; rebuilt launcher/splash resources; original 1.3.2 native base from `1e6ea7f6457306e9a12aa478f27397127caf6e39` retained. All 148 existing native libraries and seven DEX files match the previous APK byte for byte. This update is not a new full Gradle build: it adds freshly compiled `libmox_mls_jni.so` for all four ABIs and signs the rebuilt APK with the same certificate.
- Native MLS: built from current MoxCore Rust/C++ source with SQLCipher and statically bundled OpenSSL. The previous APK omitted the JNI library. The canonical `MoxChat/scripts/build-android-release.mjs` now builds/enables native MLS and fails if preparation fails. No TypeScript cryptographic fallback was added.
- Toolchain for this update: Node.js 24.18.1, Hermes from the existing Expo dependencies, Temurin Java 17.0.20.1, Android Build Tools 36.0.0, NDK 27.1.12297006, the locked Rust dependencies, and apktool 3.0.3. The original native base used Gradle 8.14.3 and CMake 3.22.1.
- Package: `com.ponzs.moxchat`, version `1.3.2`, version code `2`, target SDK `36`, minimum SDK `26` (**Android 8.0+**). ABIs: `arm64-v8a`, `armeabi-v7a`, `x86`, `x86_64`. The embedded bundle does not require Metro.
- Signing: the existing project **Android debug/test key** is retained; a production distribution key has not been configured. Updating an installed app requires the same signing key.
- UI corrections: modern four-tab navigation and current shared forms replace the obsolete drawer/pages; Discover routing, profile avatar/top spacing, search controls, composer icons/touch handling, dark send icon, keyboard avoidance, foldable/tablet layout and actual pane resizing are corrected. Content panes start equally wide after excluding the tab rail and divider.
- Relay health: session-level ownership, immediate foreground check and a three-second polling interval; overlapping checks are coalesced, timeouts are bounded, and manual/global checks use the same identity. Android checks again on foreground resume. Successful signed/authenticated health validation remains required.
- Platform isolation: Android-specific composer buttons/text field, keyboard avoidance, divider gesture and startup prelude use platform files/selection. iOS/Web retain their previous startup prelude; the additional SQLCipher OpenSSL dependency is Android-only. Shared pane geometry fixes also address the reported iPad divider defect. The pre-existing iOS project edit was preserved unchanged.
- Validation: APK ZIP integrity, same signing certificate, 16 KB ZIP alignment, all four MLS libraries and their JNI exports/ELF alignment, unchanged existing native/DEX content, TypeScript and focused lint checks. A run-owned Android 16 arm64 emulator exercised narrow/wide navigation, search, profile, system-keyboard placement, repeated voice/text switching, dark icons, actual divider movement, automatic relay health, and native MLS startup. A separate iPad simulator loaded the current JavaScript for shared-layout regression; a browser used a fresh local production Web export. Earlier Find N2 physical-device evidence concerns preceding builds; the final APK was not re-tested on physical hardware. These checks do not constitute exhaustive cross-platform or end-to-end messaging acceptance.
- Distribution: [download the updated APK](https://github.com/MoxChat/MoxChat/releases/download/v1.3.2/moxchat-android-1.3.2.apk). Web ZIP and Lazycat LPK remain the previously published packages described above; the current Web regression export was not substituted into those assets. Desktop packages are not included.
- Installation: verify SHA-256, download/open the APK, and allow installation from that source. The filename and app version remain unchanged for this replacement; compare the checksum to identify the updated file.

| Artifact | SHA-256 |
| --- | --- |
| `moxchat-web-1.3.2.zip` | `1597d701da863c1a1d6a1ca24638a30e8a8632765d70bac76809513881d74020` |
| `moxchat-1.3.2.lpk` | `62fd1b52bbb522707eb1edc69cd3394e6bc8640face5f6acf291f4fe83c60070` |
| `moxchat-android-1.3.2.apk` | `5ca3f9aa2efd06647fc7b4c09bdeaebddd3d6e41309de4ac9fcae43d1fb573d9` |
